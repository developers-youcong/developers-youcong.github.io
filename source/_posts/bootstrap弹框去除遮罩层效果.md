---
title: bootstrap弹框去除遮罩层效果
date: 2019-08-28 17:08:33
tags: "javascript"
---

是通过css解决这个问题,核心css代码如下:
<!--more-->
```
	  .modal-backdrop {
			filter: alpha(opacity=0)!important;
			opacity: 0!important;
			}

```
alpha和opacity通常是决定透明度。

alpha和opacity区别是什么?

相同点:
都是值为0表示完全透明，值为1表示完全不透明。

不同点:
alpha可以应用元素特定的属性，只能作用于当前元素，其子元素不能继承，而opacity不仅仅作用于当前元素，也会影响子元素及其子子元素，具有继承性。