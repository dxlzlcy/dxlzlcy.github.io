---
layout: post
title: learn_scrapy
---

[TOC]





### 创建一个scrapy项目

```shell
scrapy startproject aaa
cd aaa
scrapy genspider zfcg htgs.ccgp.gov.cn # 以政府采购爬虫为例，zfcg后面跟的是允许爬的域名
```

### settings.py设置

然后编辑`settings.py`

```python
# Obey robots.txt rules
ROBOTSTXT_OBEY = False

DEFAULT_REQUEST_HEADERS = {
  'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
  'Accept-Language': 'en',
  'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36',
}


```



如果配置了`pipelines.py`，还要设置

```python
ITEM_PIPELINES = {
#    'wxapp.pipelines.WxappPipeline': 300,
   'wxapp.pipelines.ZfcgPipeline': 300,
}
```

### 爬虫文件示例

然后设置爬虫`zfcg.py`，这是crawl spider的一个例子，用`scrapy genspider -t crawl zfcg htgs.ccgp.gov.cn`创建，用`Rule(LinkExtractor(allow=r'.*index_\d'), callback='parse_item', follow=True)`来过滤允许爬取的网页，可以指定回调函数

```python
# -*- coding: utf-8 -*-
import scrapy
from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import CrawlSpider, Rule

start = 16000 #可被其他文件调用
end = 18000

class ZfcgSpider(CrawlSpider):
    global start,end
    name = 'zfcg'
    allowed_domains = ['htgs.ccgp.gov.cn']
    start_urls = ['http://htgs.ccgp.gov.cn/GS8/contractpublish/index_%s'%(i+1) for i in range(start,end)]

    rules = (
        Rule(LinkExtractor(allow=r'.*index_\d'), callback='parse_item', follow=True),
        Rule(LinkExtractor(allow=r'.+GS8/contractpublish/detail.+'), callback='parse_detail', follow=False),
    )

    def parse_item(self, response):
        print(response.url)
        print('='*40)
        item = {}
        return item

    def parse_detail(self,response):
        item = {'url':response.url}

        title = response.css('body > div.vT_z.w900 > div.vT_detail_main > div.vT_detail_header > h2::text').get()
        item['title'] = title

        for tr in response.css('tr'): 
            a = tr.xpath('.//text()').getall()
            b = [i.strip() for i in a if i.strip()]
            if len(b)<2:
                b.append('')
            item[b[0].replace('：','')] = b[1]
        
        yield item
        # print(item)
```

### scrapy的css选择器用法

```python
title = response.css('body > div.vT_z.w900 > div.vT_detail_main > div.vT_detail_header > h2::text').get()
href = response.css('h2::attr(href)').get()
```

### pipelines.py示例

```python
from scrapy.exporters import JsonLinesItemExporter, CsvItemExporter,JsonItemExporter
from wxapp.spiders import zfcg


class WxappPipeline(object):
    def __init__(self):
        self.fp = open("weixin_jiaocheng.json", "wb")
        self.exporter = JsonLinesItemExporter(
            self.fp, ensure_ascii=False, encoding="utf8"
        )

    def process_item(self, item, spider):
        self.exporter.export_item(item)
        return item

    def close_spider(self, spider):
        self.fp.close()


class ZfcgPipeline(object):
    def __init__(self):
        print("pipeline open")
        print("*" * 30)
        self.fp = open("zfcg_%s-%s.json" % (zfcg.start, zfcg.end), "wb")
        # self.exporter = CsvItemExporter(self.fp,encoding='utf8')
        self.exporter = JsonLinesItemExporter(self.fp, encoding="utf8")

    def process_item(self, item, spider):
        self.exporter.export_item(item)
        print("exporting")
        print("*" * 30)
        return item

    def close_spider(self, spider):
        self.fp.close()
        print("pipeline close")
        print("*" * 30)
```

