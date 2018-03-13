---
title: 简单的css3渐变背景
date: 2018-03-13 14:30:15
tags: ['CSS3']
categories: ['CSS']
---

### html
``` html
<div class="dynamic-shadow-parent">
  <div class="dynamic-shadow"></div>
</div>
```

### css
``` css
.dynamic-shadow-parent {
  position: relative;
  z-index: 1;
}
.dynamic-shadow {
  position: relative;
  width: 10rem;
  height: 10rem;
  background: linear-gradient(75deg, #6d78ff, #00ffb8);
}
.dynamic-shadow::after {
  content: '';
  width: 100%;
  height: 100%;
  position: absolute;
  background: inherit;
  top: 0.5rem;
  filter: blur(0.4rem);
  opacity: 0.7;
  z-index: -1;
}

```
### 效果
![css3-linear-gradient](https://raw.githubusercontent.com/wangmingdong/docImg/master/css3-indent.png)
