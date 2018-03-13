---
title: 简单的css3加载动画
date: 2018-03-13 13:50:37
tags: ['CSS3']
categories: ['CSS']
---
### html
``` html
<div class="donut"></div>
```

### css
``` css
@keyframes donut-spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.donut {
  display: inline-block;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-left-color: #7983ff;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: donut-spin 1.2s linear infinite;
}

```
### 效果
![css3-load](https://raw.githubusercontent.com/wangmingdong/docImg/master/css3-load.gif)
