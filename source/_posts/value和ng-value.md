---
title: value和ng-value
date: 2017-02-23 16:01:37
tags: angular
---

开年第一篇，前两周赶进度特别忙，基本重构了整个系统，也踩了不少的坑，其中就有angular指令ng-value和value这俩货
>类似的还有ng-disabled和disabled等

# ng-value&&value #

# ng-disabled&&disabled

上来先举个正确的例子

    ng-disabled="!(address.selected&&filterOptions.filterText.endTime&&filterOptions.filterText.startTime)"

>这个是账户系统中的原例，注意逻辑关系的书写

~~ng-disabled="{{associate.JobStatus == 2}}"~~

上面一条是错的

>注意disable存在时，表单不会对其校验，可使用visible属性

# ng-if #

这里要特别注意ng-if和ng-show的区别

ng-if = false 时，相应的标签元素在调试器里消失（不知道是否被移出了dom），且不占空间。而
ng-show = false 时 相应的的元素还是在dom中的，

如下
* ng-if 在后面表达式为 true 的时候才创建这个 dom 节点，ng-show 是初始时就创建了，用 display:block 和 display:none 来控制显示和不显示。
* ng-if 会（隐式地）产生新作用域，ng-switch 、 ng-include 等会动态创建一块界面的也是如此。 