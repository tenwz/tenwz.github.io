---
layout: post
title: 列表推导







---

“事实上，列表推导和SQL之间的高度相似性并不是一个奇怪的事情，因为他们都是基于数学里的集合论”

微言大义。

[关于王垠对数据库的理解](https://www.zhihu.com/question/329153374/answer/723485238)

这篇回答下的评论很值得一看，很多人对于数据库的理解往往只停留在表面上，并没有深究其本质

我认为学习计算机最好的方法，就是去学习一门语言，下面是关于列表推导的一些例子。

列表推导是一种能够生成元素并对生成的元素做测试的表达式，格式如下

```era
[Exp || Gen,Tests,Gen,Tests]
```

其中，Generator 的格式为

```erlang
X <- [2,3,5,7,11]
```

Tests 可有可无，它们是一组布尔表达式，针对值和绑定变量的每一种组合进行求值。如果同一组内的全部 Tests 都返回 true，则从当前的绑定变量和它们的值上生成表达式。

两个例子

```erlang
filter(P,Xs) -> [X || X <- Xs, P(X) ].
```

```erlang
1> [Element || Element <- list:seq(1,4) , Element rem 2 == 0] . 
[2,4]
```



### 快排



```erlang
qsort([]) -> [];
qsort([Pivot|T]) -> 
  qsort([X || X <- T,X < Pivot]) ++ [Pivot] ++ qsort([X || X <- T,X >= Pivot]).
```





不得不说，模式匹配加列表推导真的很强大



|

|

---

### 勾股数组

pythag(N) 函数生成一个包含所有符合勾股定理的 {A,B,C} 组合的列表

```erlang
pythag(N) -> [{A,B,C} || A <- list:seq(1,N),B <- list:seq(1,N),C <- list:seq(1,N),
             A+B+C =< N,
             A * A + B * B =:= C * C].
```





|

|



```erlang

1> lib_misc:pythag(16). 
[{3,4,5},{4,3,5}]
```



|

|

|

怀疑 erlang 和 prolog 存在着联系。另外感觉这种写法性能也不会太好。