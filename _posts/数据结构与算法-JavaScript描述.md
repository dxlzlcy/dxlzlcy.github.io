[视频地址](https://www.bilibili.com/video/av60711313)

[557](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/submissions/)

```javascript
var reverseWords = function(s) {
    let arr = s.split(' ');
    let res = arr.map(item => {
        return item.split('').reverse().join('');
    })
    return res.join(' ')
};
```

