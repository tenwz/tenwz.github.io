---
layout: post
title: Continuation-passing style
---
#  Continuation-passing style

```scheme
(define (map f xs)
  (if (empty? xs) '()
      (cons (f (first xs)) (map f (rest xs)))))
(map (λ (x) (+ x 1)) '(1 2 3))
```

```scheme
(define (map-k f xs k) 
  (if (empty? xs) (k '()) 
      (f (first xs) (λ (v) (map-k f (rest xs) (λ (rest-v) (k (cons v rest-v)))))))) 
(map-k(λ (x k) (k (+ x 1))) '(1 2 3) (λ (x) x))
```

可以看到，在 map-k 中，所有的函数调用都是尾递归，也就不存在由递归引起的 stack 空间的消耗（在支持tail-call 优化的语言中）

---

把 continuation 作为 first class 进行操作是 Scheme 和其它语言很大的不同。

Scheme 提供了 call-with-current-continuation(call/cc) 处理 continuation.

Scheme 可以把当前的 continuations 作为一个函数保存起来，当以 后调用这个函数的时候，就可以回到函数执行中的任何一个时刻。这 是非常强大的特点，我们可以利用这个特点实现非常复杂的控制结构， 人工智能的分支剪切，回溯，线程，coroutines, ...

---

🧙‍♂️练习：

<u>阴阳谜题</u>

由David Madore提出:

```scheme
(let* ((yin ((lambda (foo) (display "@") foo) (call/cc (lambda (bar) bar))))
       (yang ((lambda (foo) (display "*") foo) (call/cc (lambda (bar) bar)))))
  (yin yang))
```

请试着推导一下输出。

