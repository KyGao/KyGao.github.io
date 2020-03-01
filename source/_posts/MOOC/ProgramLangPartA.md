---
title: Programming Language (Part A)
tags:
  - Coursera
  - ML
  - MOOC
categories:
  - MOOC
date: 2020-02-14 13:10:43
description: Coursera上据说很有挑战的一门课，课程目的是学会如何设计优美的程序结构，掌握对编程语言的统一宏观概念。在Part A中利用ML语言和Emacs编辑器作为工具，Part B 和 Part C分别是 Rocket 和 Ruby。
---

## Week 1: Introduction

### Welcome to this course

- Initial Motivation

This course is neither particularly theoretical nor just about programming specifics -- it will give you **a framework for understanding how to use language constructs effectively and how to design correct and elegant programs**. By using different languages, you will learn to think more deeply than in terms of the particular syntax of one language. The emphasis on functional programming is essential for learning how to write **robust, reusable, composable, and elegant programs**. 

- Recommended Background

  - Variables, if, loops, arrays
  - Recursion (递归)
  - Implementation vs. Interface
  - linked lists, binary tree
  - Dynamic-dispatch
  - Python or Javascript

- Why Part A, Part B, Part C
- 
  - huge contents
  - reasonable stopping point

- Grading Policy

  - Auto-Grader Policy (resubmit at most once per day (must exceed 80%))
  - Challenge Problem
  - Peer assesment
  - Exams (resubmit at most once per day (must exceed 80%))

- Course Overview

  - **Part A**

  0. Software Installation
  1. Basics, functions, recursion, scope, variable, tuples, lists, ...
  2. Datatypes, pattern-matching, tail recursion
  3. First-class functions, closures
  4. Type inference, modules

  - **Part B**

  5. Quick "re-do" in a dynamically typed language
  6. Implementing languages with interpreters (Static vs. dynamic typing)

  - **Part C**

  7. Dynamically-typed Object-Oriented Programming
  8. OOP vs. Functional decomposition

### Software Installation and Test

*OS: Manjaro (Linux)*

1. **Emacs**
    - **Installation**:  `yay -Sy emacs`
    - **Usage**: check over [here](/2020/02/tools/Emacs%20tutorial%20笔记/)
2. **SML/NJ**
    - **Installation**
    follow the step [here](http://www.smlnj.org/dist/working/110.80/INSTALL) , quite easy but remember to `sudo`for all that commands .  
      My Version is **110.87**
    - **Test**
add the path like: `export PATH="/usr/share/smlnj/bin:$PATH"` (in .zshrc)
    type `sml` and then `1+1;` to check if it works

3. SML Mode for Emacs
    - **Installation**
    In emacs: `M-x package-install RET sml-mode RET`
    add SML/NJ  into ~/.emacs: 
    `(setenv "PATH" (concat "/usr/share/smlnj/bin:" (getenv "PATH")))
      (setq exec-path (cons "/usr/share/smlnj/bin"  exec-path))`
    - **Test**
      1. `C-x C-f test.sml` to create a new sml file
      2. `val n = 1;` to see if it's high-lightened
      3. `C-c C-s <Return>` to see if emacs open a new buffer as the SML prompt
      4. `1 + 1;` in the prompt to see if it is working
      5. `M-p` to see history commands
      6. `C-d` to end the prompt normally
      
### Homework
​		第一周的作业被官方称为 "fake assignment" , 只是为了让我们熟悉提交作业的流程。以后每次作业都需要自己编写 `test.sml` 来测试作业的完成度，这个方式还是挺新鲜的。举个例子，这一周的作业就是在 `hw0provided.sml` 里面改一个符号，来实现乘幂的函数。但是我们又不能直接测试这个函数，所以官方提供了另一个 `hw0test.sml` 来辅助检测我们作业完成的正确性。Assignment中也提到了在以后的作业中，需要我们自己考虑边界情况进行测试，他们不会直接提示，只会通过 auto-grader 来检测我们代码的 robustness。
​		在此有一个tricks可以分享一下，在用emacs编辑的时候，可以开三个窗口（其中`C-x 2` 和 `C-x 3`分别是上下镜像和左右镜像），右边窗口是REPL，左边是两个窗口，上面是函数，下面是test，看起来就很舒服了

---
## Week 2

### Welcome Message
这一周先学习各种数据类型，然后学习函数的"scope"和"shadowing"概念，接着学习"pairs"和"list"来组合成复合数据类型，再然后学习"let expressions"。那就开始吧！

### ML Variable Bindings and Expressions
- **static environment**: 记录数据的类型 (Type)
  **dynamic environment**: 记录数据的值 (Value)
  程序运行之前，需要在 static environment 中进行一次 "Type check"，通过之后才会运行程序 ("Evaluation")，即更新dynamic environment

- **A variable binding**
  ```ML
  val x = [expression]
  ```
  一个 variable（就是程序中的 `val`）会受到3个约束：
  1. 等号右边的 expression 的 syntax 要正确
  2. static environment 中的 type-check 要通过
  3. dynamic environment 中的 evaluation 要满足

### Rules for Expressions
  每个expression同样也会受到上面三种约束 (Expression bindings)，例如if .. then .. else .. 语句：

doesn't typecheck

### The REPL and Errors
- **REPL**
  Read-Evaluate-Print-Loop
  就是在 `use` 调用sml的时候进行的操作，调试很方便，但是注意每次 use 的时候都要重启REPL（C-d C-c C-s）
  
- Errors
  4种错误：syntax, type-check, evaluation, result
  编译器会先查看是否出现 syntax error (if then else的完整性 / 变量命名是否用到了关键词等等)，在解决之后再会来检查 type-check (-5要用~5表示 / 4/5要用4 div 5表示等等)，然后程序会开始运行，继续检查在 evaluation 的时候出的错 (除数不能为0等等)。接着就是程序运行通过了，但是结果不如我们的意，这时候就需要我们自己去查看代码发现问题了。
  
###  Shadowing
  当变量被重新赋值的时候，原来的值就被shadowing了，在REPL原赋值语句的输出也会变成 <hidden-value>，所以dynamic environment就是指在程序运行时也是dynamic的

### Function
- **Function bindings**
  - **A function is a value**: 当我们调用func的时候才开始对func进行evaluate，一直可以把func当作值为0的value
  - 函数的返回值类型由程序自动赋给（程序通过recursion得到）

- Unless a function has exactly one argument, you need to use parentheses (括号) to call it
- 
- Type syntax 中表示函数的type时会用 * 来表示and，比如上面的函数type就是 `int * int -> int`

- 所有编程要用到 Loop 的都可以用 Recursion 来替代

- **Calling the Function**
  调用函数时也要检查这三样东西：Syntax, Type-checking and Evaluation
  - Syntax: `e0 (e1,...,en)` （ML不支持可选参数）
  - Type-cheking
  检查两个东西，一个是函数参数的数据类型是否和定义的相同，一个是函数名的数据类型( `(t1 * ,,, * tn) -> t` )是否和定义相同，如果这两点满足了，就将数据类型t赋给 e0(e1,..,en)
  - Evaluation
    1. evaluate e0. 找到 e0 这个函数; 
    2. evaluate all arguments. 比如2+2的参数要算成4;
    3. evaluate the function. 将参数代进去是否能够运行函数（会新开一片区域放临时变量）; 


- **Pairs and Tuples**
  - Pairs (2-tuples)
    - Syntax: `(e1, e2)
    - Type-cheking: `ta * tb`
    - Evaluate: e1, e2分别赋两个值可以运行
  - Tuples
    - 就是不只两个元素的pair
  - Nesting
    - tuples很重要的用法，可以tuples嵌套tuples，取出其中的元素就用 `#1 x1` 来取出x1这个tuple的第一个元素，甭管什么类型都可以取

- **Lists**
  - lists和tuples的区别
    lists可以含有任意数量的元素，但是必须是同一类型
    tuples可以拥有不同数据类型，但是只能含有有限数量的元素
  - 用法：
    <table>
      <tr>
        <td><b>构造</b></td>
        <td>`[6, 5, 1]`</td>
        <td>`6::5::1;`</td>
      </tr>
      <tr>
        <td><b>检查为空</b></td>
        <td>`null x` </td>
        <td>返回true则为空 ->bool</td>
      </tr>
      <tr>
        <td><b>掐头</b></td>
        <td>`hd x` </td>
        <td>返回第一个元素 ->a</td>
      </tr>
          <tr>
        <td><b>取尾</b></td>
        <td>`tl x` </td>
        <td>返回除了第一个元素的所有元素 ->a list</td>
      </tr>
    </table>
  - type-checking: 只能是同一种数据类型，但是又可以是嵌套的数据类型。空list是alpha类型，可以变成任意类型（所以null实际上是检测是否为alpha类型）
  - function example
    ```java
    fun append (xs : int list, ys : int list) = 
        if null xs
        then ys
        else (hd xs) :: append((tl xs), ys)
        
    fun sum_pair_list (xs : (int * int) list) =
        if null xs
        then 0
        else #1 (hd xs) + #2 (hd xs) + sum_pair_list(tl xs)
    ```

- **Let expressions**
  - Syntax: `let b1 b2 ... bn in e end` 返回类型由 e 决定
  - 创建局部作用域 (*scope*)，在bi中定义的值出了lei就不复存在
  - **Nested Functions**: 将函数内嵌在let语句中，使得函数变为局部函数
    ```java
    fun countup_from1 (x : int) =
        let
            fun count (from : int, to : int) =
                if from=to
                then to::[]
                else from :: count(from+1, to)
        in
            count(1, x)
        end
        
    // 甚至更为简洁
    fun countup_from1 (x : int) =
        let
            fun count (from : int) =
                if from=x
                then x::[]
                else from :: count(from+1)
        in
            count(1)
        end
    ```
  - Note: **Avoid repeated recursion**

- **Options**
  允许返回不同类型的函数
      <table>
      <tr>
        <td rowspan="2"><b>构造</b></td>
        <td>`NONE`</td>
        <td>type: 'a option</td>
      </tr>
      <tr>
        <td>`SOME`</td>
        <td>type: t option</td>
      </tr>
      <tr>
        <td rowspan="2"><b>调用</b></td>
        <td>`isSome`</td>
        <td>type: 'a option -> bool</td>
      </tr>
      <tr>
        <td>`valOf`</td>
        <td>type: 'a potion -> 'a</td>
      </tr>
    </table>

- **Booleans and Comparison Operations**
  - e都必须要是bool类型：`e1 andalso e2`  `e1 orelse`  `not e`
  - comparison两边必须是同类型(int就不等于real): `=` `<>` `>` `<` `>=` `<=`
  - 对于real类型不能使用 `=` 和 `<>`来比较，因为real本来就不准确
 
- **No Mutation**
  `reference=alias vs. copy`
  ML是不允许mutation的（不知道是不是指静态语言的意思），因此，不需要考虑aliases和copies的区别（指向的存储空间的存储类型不会发生冲突），像java这种编程语言就需要考虑aliases，但都有好有坏。所以Java在安全性上会做的更好：比如在登录注册的时候，数据库调用已有的用户信息与当前登录信息进行匹配，如果得到的是alias，就很容易通过客户端修改数据库，但如果客户端得到的是copy，就无法篡改数据库
  
- **Five different things** (shouldn't confused)
  1. *Syntax*: how
  2. *Semantics*: mean
  3. *Idioms*: typical patterns
  4. *Libraries*: standard facilities
  5. *Tools*: implementations
  2 和 3是这门课侧重的