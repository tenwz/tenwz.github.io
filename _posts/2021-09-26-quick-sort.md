---
layout: post
title: 用列表推导实现的快排








---

```erlang
qsort([]) -> [];
qsort([Pivot|T]) -> 
  qsort([X || X <- T,X < Pivot]) ++ [Pivot] ++ qsort([X || X <- T,X >= Pivot]).
```





不得不说，模式匹配加列表推导真的很强大



|

|

|

|

---

#### 勾股数组

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