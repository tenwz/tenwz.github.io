---
layout: post
title: Y 组合子









---

什么是 Y 组合子？

![y](https://img1.doubanio.com/view/status/l/public/bc09fb7f07f253a.jpg)







这是一篇可以开眼界，但是可能没有什么用的文章。

我们知道递归函数，我们也知道匿名函数，但是你知道匿名的递归函数吗？

｜

｜

lambda 演算是一种极其优美的计算模型，如果你了解过 SICP，我相信你一定会感叹 lambda演算 的神奇

cons,car,cdr 可以用 lambda 表示

延迟计算模型 delay force 可以用lambda 表示

就连数字1,2,3,4...也可以用 lambda表示

并且这些语言大厦的底层模型相当简洁优雅，想想 Java 8 复杂的函数式语法吧。

｜

“一个‘对象’就是个带了一堆状态的闭包，有什么难理解的”		——开源酱

我深以为然，在函数式的世界里，面向对象没有什么魔法。

｜

今天我要展示的是一种更神奇的思想，Y 组合子

lambda 演算中没有“命名”这个概念，因此写递归函数的时候需要一点技巧，Y 组合子就是这种实现这种技巧的一个函数，它让 lambda 演算中的函数可以递归，从另一方面证明了 Lambda 演算中的“应用”和“命名”是“等价”的

在正统的 Lambda 演算里函数全部是没有名字的，因此[递归函数](https://www.zhihu.com/search?q=递归函数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A23893046})无法实现，考虑
![[公式]](https://www.zhihu.com/equation?tex=%5Cmathrm%7Bfib%7D%3D%5Clambda+x.%5Cmathrm%7Bif%7D%28x%3E0%29%5C%2C%5Cmathrm%7Bthen%7D%5C%2C%28x+%5Ctimes+%28%5Cmathrm%7Bfib%7D%5C%2C%28x-1%29%29%29%5C%2C%5Cmathrm%7Belse%7D%5C%2C%281%29)
完美！

于是我们现在惟一的任务就是写出一个闭合的 Lambda [表达式](https://www.zhihu.com/search?q=表达式&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A23893046})和 Y 等效，所幸 Haskell B. Curry 找到了一个：
![[公式]](https://www.zhihu.com/equation?tex=Y+%3D+%5Clambda+f.%28%5Clambda+x+.f%5C%2C%28x%5C%2Cx%29%29%28%5Clambda+x+.f%5C%2C%28x%5C%2Cx%29%29)
试试调用它（在传名调用下）：
![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Beqnarray%7D%0AY%5C%2Cg+%26+%3D+%26+%28%5Clambda+x.+g%5C%2C%28x%5C%2Cx%29%29%28%5Clambda+x.+g%5C%2C%28x%5C%2Cx%29%29%5C%5C%0A%26%3D%26+g+%5C%2C%28%28%5Clambda+x.+g%5C%2C%28x%5C%2Cx%29%29%28%5Clambda+x.+g%5C%2C%28x%5C%2Cx%29%29%29+%5C%5C%0A%26%3D%26++g%5C%2C%28Y%5C%2Cg%29%0A%5Cend%7Beqnarray%7D)
完美！

