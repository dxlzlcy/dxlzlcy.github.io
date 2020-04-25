### pandoc+md写slides

示例

```
---
theme:
- Copenhagen

---



# Landslide

------

# Overview

Generate HTML5 slideshows from markdown, ReST, or textile.

Landslide is primarily written in Python, but it's themes use:

- HTML5
- Javascript
- CSS

------

# Code Sample

Landslide supports code snippets

​```python
!python
def log(self, message, level='notice'):
    if self.logger and not callable(self.logger):
        raise ValueError(u"Invalid logger set, must be a callable")

    if self.verbose and self.logger:
        self.logger(message, level)
​```
```

然后执行

```bash
pandoc test.md -t beamer -o test.pdf
```



## 对中文的支持

要利用tex模板功能，首先，新建一个default模板

```bash
pandoc -D latex > default.tex
```

打开`default.tex`

找到这几行，删掉

```tex
$if(CJKmainfont)$
  \ifxetex
    \usepackage{xeCJK}
    \setCJKmainfont[$for(CJKoptions)$$CJKoptions$$sep$,$endfor$]{$CJKmainfont$}
  \fi
$endif$
```

改成这几行

```tex
\usepackage{xeCJK}
\setCJKmainfont[BoldFont=STHeiti,ItalicFont=STKaiti]{STSong}

```

保存，然后在md文件的开头处添加

```yaml
---
CJKmainfont: STSong
CJKoptions:
  - BoldFont=STHeiti
  - ItalicFont=STKaiti
---
```

搞定，编译时加上一些参数即可

```bash
pandoc -t beamer -s --pdf-engine=xelatex --template=default.tex test.md -o test.pdf
open test.pdf
```

参考：

- [pandoc does not recognize Chinese characters](https://stackoverflow.com/questions/40892725/pandoc-does-not-recognize-chinese-characters)



### reveal-md

~~~markdown
# 上市公司治理
## 副标题

锦瑟无端五十弦，一弦一柱思华年。
庄生晓梦迷蝴蝶，望帝春心托杜鹃。
沧海月明珠有泪，蓝田日暖玉生烟。
此情可待成追忆，只是当时已惘然。

---

# latex 模板
## 副标题

```
\documentclass{article} 

\usepackage{ctex}

\begin{document}


    
\end{document}
```


~~~

