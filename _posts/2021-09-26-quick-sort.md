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