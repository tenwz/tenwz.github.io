---
layout: post
title: A Little Interpreter
---



学习实现语言，最好从最简单，最干净的语言开始，迅速写出一个可用的解释器。之后再逐步往里面添加特性，同时保持正确。这样才能有条不紊地构造出复杂的解释器。

我们采用scheme语言。

我们将实现一个简单的解释器，名叫“R2”。它可以作为一个简单的计算器用，并且具有变量定义，函数定义和调用功能。

------

#### 预备知识

scheme语言，模式匹配，S表达式，递归，闭包，抽象语法树

------

#### 计算器

你将可以实现以下功能

```scheme
(calc '(+ 1 2))
;; => 3
(calc '(* 2 3))
;; => 6
(calc '(* (+ 1 2) (+ (/ 6 2) 4)))
;; => 21
```

```scheme
#lang racket                                  ; 声明用 Racket 语言

(define calc
  (lambda (exp)
    (match exp                                ; 分支匹配：表达式的两种情况
      [(? number? x) x]                       ; 是数字，直接返回
      [`(,op ,e1 ,e2)                         ; 匹配提取操作符op和两个操作数e1,e2
       (let ([v1 (calc e1)]                   ; 递归调用 calc 自己，得到 e1 的值
             [v2 (calc e2)])                  ; 递归调用 calc 自己，得到 e2 的值
         (match op                            ; 分支匹配：操作符 op 的 4 种情况
           ['+ (+ v1 v2)]                     ; 如果是加号，输出结果为 (+ v1 v2)
           ['- (- v1 v2)]                     ; 如果是减号，乘号，除号，相似的处理
           ['* (* v1 v2)]
           ['/ (/ v1 v2)]))])))
```

------

### R2: 一个很小的程序语言

你将实现以下功能：

- 变量：x
- 函数：(lambda (x) e)
- 绑定：(let ([x e1]) e2)
- 调用：(e1 e2)
- 算术：(• e2 e2)

需要注意的是：跟一般语言不同，我们的函数只接受一个参数。这不是一个严重的限制，因为在我们的语言里，<u>函数可以被作为值传递</u>，也就是所谓“<u>first-class function</u>”。所以你可以用嵌套的函数定义来表示有两个以上参数的函数。

<u>下面是演示例子，代码行数很少，但是它已经完全可以处理一个相当强大的函数式语言。</u>

```scheme
(r2 '(+ 1 2))
;; => 3

(r2 '(* 2 3))
;; => 6

(r2 '(* 2 (+ 3 4)))
;; => 14

(r2 '(* (+ 1 2) (+ 3 4)))
;; => 21

(r2 '((lambda (x) (* 2 x)) 3))
;; => 6

(r2
'(let ([x 2])
   (let ([f (lambda (y) (* x y))])
     (f 3))))
;; => 6

(r2
'(let ([x 2])
   (let ([f (lambda (y) (* x y))])
     (let ([x 4])
       (f 3)))))
;; => 6
```

------

#### 对基本算术操作的解释与实现

```scheme
(match exp
  ... ...
  [`(,op ,e1 ,e2)
   (let ([v1 (interp e1 env)]             ; 递归调用 interp 自己，得到 e1 的值
         [v2 (interp e2 env)])            ; 递归调用 interp 自己，得到 e2 的值
     (match op                            ; 分支：处理操作符 op 的 4 种情况
       ['+ (+ v1 v2)]                     ; 如果是加号，输出结果为 (+ v1 v2)
       ['- (- v1 v2)]                     ; 如果是减号，乘号，除号，相似的处理
       ['* (* v1 v2)]
       ['/ (/ v1 v2)]))])
```

------

#### 对数字的解释与实现

```scheme
[(? number? x) x]
```

------

#### 对变量、函数的解释与实现

- 对变量最基本的操作，是对它的“绑定”（binding）和“取值”（evaluate）

- 我们通过“环境”，来记录变量的值，并且把它们传递到变量的“可见区域”。变量的可见区域，用术语说叫做“作用域”（scope）
- 环境是一个堆栈（stack）。内层的绑定，会出现在环境的最上面，这就是在“压栈”。这样我们查找变量的时候，会优先找到最内层定义的变量。
- 环境被扩展以后，形成了一个新的环境，而原来的环境并没有被改变。当我们扩展一个环境，进入递归，返回之后，外层的代码必须仍然可以访问原来外层的环境。

**环境的解释与实现**

```scheme
;; 空环境
(define env0 '())

;; 对环境 env 进行扩展，把 x 映射到 v
(define ext-env
  (lambda (x v env)
    (cons `(,x . ,v) env)))

;; 取值。在环境中 env 中查找 x 的值
(define lookup
  (lambda (x env)
    (let ([p (assq x env)])
      (cond
       [(not p) #f]
       [else (cdr p)]))))
```

你需要理解这个例子

```scheme
(let ([x 1])         ; env='()。绑定x到1。
  (let ([y 2])       ; env='((x . 1))。绑定y到2。
    (let ([x 3])     ; env='((y . 2) (x . 1))。绑定x到3。
      (+ x y))))     ; env='((x . 3) (y . 2) (x . 1))。查找x，得到3；查找y，得到2。
;; => 5
```



变量的解释与实现

```scheme
[(? symbol? x) (lookup x env)]			;在环境中，沿着从内向外的“作用域顺序”，查找变量的值
```



绑定的解释与实现

```scheme
[`(let ([,x ,e1]) ,e2)                           
 (let ([v1 (interp e1 env)])              ; 解释右边表达式e1，得到值v1
   (interp e2 (ext-env x v1 env)))]       ; 把(x . v1)扩充到环境顶部，对e2求值
```



**深刻理解作用域**

- 你需要理解为什么此处的答案是 6 （Lexical Scoping ）而不是 12 （Dynamic Scoping）
- 你需要理解两种不同的设计会带来怎样的影响

```scheme
(let ([x 2])
  (let ([f (lambda (y) (* x y))])
    (let ([x 4])
      (f 3))))

;; => 6
```



对函数的解释与实现

- 为了实现 lexical scoping，我们必须把函数做成“闭包”（closure）。

- 闭包是一种特殊的数据结构，它由两个元素组成：函数的定义和当前的环境。

- ```scheme
  (struct Closure (f env))
  ```

```scheme
;;对 (lambda (x) e) 的解释
[`(lambda (,x) ,e)
 (Closure exp env)]
```

------

### 对调用的解释

```scheme
[`(,e1 ,e2)                                            
 (let ([v1 (interp e1 env)]             ; 计算函数 e1 的值
       [v2 (interp e2 env)])            ; 计算参数 e2 的值
   (match v1
     [(Closure `(lambda (,x) ,e) env-save)      ; 用模式匹配的方式取出闭包里的各个子结构
      (interp e (ext-env x v2 env-save))]))]    ; 在闭包的环境env-save中把x绑定到v2，解释函数体     

```

你需要思考：

<u>如果把 env-save 改成 env 函数的调用会发生怎样的变化？</u>

<u>早期的 Lisp 语言，比如 Emacs Lisp，都使用 dynamic scoping，为什么会这样？</u>

<u>环境使用函数式数据结构的好处是什么？</u>

------

### 完整代码

```scheme
#lang racket

;;; 以下三个定义 env0, ext-env, lookup 是对环境（environment）的基本操作：

;; 空环境
(define env0 '())

;; 扩展。对环境 env 进行扩展，把 x 映射到 v，得到一个新的环境
(define ext-env
  (lambda (x v env)
    (cons `(,x . ,v) env)))

;; 查找。在环境中 env 中查找 x 的值。如果没找到就返回 #f
(define lookup
  (lambda (x env)
    (let ([p (assq x env)])
      (cond
       [(not p) #f]
       [else (cdr p)]))))
       
;; 闭包的数据结构定义，包含一个函数定义 f 和它定义时所在的环境
(struct Closure (f env))

;; 解释器的递归定义（接受两个参数，表达式 exp 和环境 env）
;; 共 5 种情况（变量，函数，绑定，调用，数字，算术表达式）
(define interp
  (lambda (exp env)
    (match exp                                          ; 对exp进行模式匹配
      [(? symbol? x)                                    ; 变量
       (let ([v (lookup x env)])
         (cond
          [(not v)
           (error "undefined variable" x)]
          [else v]))]      
      [(? number? x) x]                                 ; 数字
      [`(lambda (,x) ,e)                                ; 函数
       (Closure exp env)]
      [`(let ([,x ,e1]) ,e2)                            ; 绑定
       (let ([v1 (interp e1 env)])
         (interp e2 (ext-env x v1 env)))]
      [`(,e1 ,e2)                                       ; 调用
       (let ([v1 (interp e1 env)]
             [v2 (interp e2 env)])
         (match v1
           [(Closure `(lambda (,x) ,e) env-save)
            (interp e (ext-env x v2 env-save))]))]
      [`(,op ,e1 ,e2)                                   ; 算术表达式
       (let ([v1 (interp e1 env)]
             [v2 (interp e2 env)])
         (match op
           ['+ (+ v1 v2)]
           ['- (- v1 v2)]
           ['* (* v1 v2)]
           ['/ (/ v1 v2)]))])))

;; 解释器的“用户界面”函数。它把 interp 包装起来，掩盖第二个参数，初始值为 env0
(define r2
  (lambda (exp)
    (interp exp env0)))
```





------

一点魔法🪄

```scheme
(let ([x e1]) e2) 
```

与以下完全等价。

```scheme
((lambda (x) e2) e1)
```

