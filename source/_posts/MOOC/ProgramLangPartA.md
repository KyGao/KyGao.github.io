---
title: Programming Language (Part A)
tags:
  - Coursera
  - ML
  - MOOC
categories:
  - MOOC
date: 2020-02-14 13:10:43

---

Coursera上据说很有挑战的一门课，课程目的是学会如何设计优美的程序结构，掌握对编程语言的统一宏观概念。在Part A中利用ML语言和Emacs编辑器作为工具，Part B 和 Part C分别是 Rocket 和 Ruby。
<!--more-->

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
  
---

## Week 3: Building Compound Types

### Building Compound Types
分清两个概念：
- base types: `int` `bool` `unit` `char`
- compound types: `tuples` `lists` `options`
三个意识：
- **Each of**: contains every
- **One of**: contains one of
- **Self reference**: refer
> eg: tuples: each  //  options: one of  // lists: all three

### Records
  ```java
  val x = {bar=(1+2, true andalso true), foo=3+4, baz=(false,9)};
  // type:{bar:int * boot=l, baz:bool * int, foo:int}
  #bar x
  // return (3, true)
  ```
- By name vs. by position (tuple好还是record好)
  tuples会更短，但是当参数多的话record更容易记也更容易理解

### Tuples as Syntactic Sugar
事实上，tuples只是records的特例:
  ```java
  // try using number as the name (reference)
  val use_num = {3="hi", 1=true}
  // return val use_num = {1:true, 3="hi"} : {1:bool, 3:string}
  
  // but when the number is in a continuous sequence
  val strange_thing = {3:"hi", 1=true, 2=3+2};
  // return val strange_thing = (true, 5, "hi") : bool * int * string
  ```
- Syntactic sugar
  make the language sweeter: simplify *understanding* and *implementing* the language. another example: `andalso` `orelse`

### Datatype Bindings
  ```java
  // "one of" 
  datatype mytype = TwoInts of int * int
                  | Str of string
                  | Pizza
                  
  // these are all legal usage, all the types are "mytype"
  val a = Str "hi"  // type: mytype
  val b = Str  // is a programming error, but it does type check
  val c = Pizza  // Pizza is a value and its type is mytype
  val d = TwoInts(1+2, 3+4)
  VAL e = a
  ```
  以上构造了一个新的type: `mytype`, 构造了三个constructors：`TwoInts` `Str` `Pizza`, 在变量定义时只需要满足其中任何一个类型即可，并且即使满足了该类型，该变量的类型依然是mytype

### Case Expression
ML 使用了两种方法来处理 one-of 的类型：`case expression` and `pattern-matcing`，后者会更强大，在这里先学习 case：
  ```java
  // mytype 的定义方式同上
  fun f (x : mytype) =   // 函数功能: mytype -> int
      case x of
          Pizza => 3
        | Str s => 8
        | TwoInts(i1, i2) => i1 + i2
  
  // usage
  f Pizza;      // return 3
  f (Str "hi"); // return 8
  //f "hi"      // error, not mytype
  f (TwoInts (7,9)); // return 16
  ```
- 就像在不断讨论if else if else
- case 必须要讨论完整，**不能漏项**也**不能重复冗余**
- 像这样同一类型但是参数不同的各种个体就叫做 **Patterns** , 不同Pattern不能重名

### Useful Datatypes
  ```java
  // Use as enumeration
  datatype suit = Club | Diamond | Heart | Spade
  datatype rank = Jack | Queen | King | Ace | Num of int
  // Then we can combine these 2 types into (suit * rank) type
  
  // Use as identifying
  datatype id = StudentNum of int
              | Name of string 
                        * (string option)
                        * string      // Name的三个string表示的是firstName (secondName) thirdName
  ```
- 关于学生的id和姓名是要用 one-of (datatype) 还是each-of (tuple) 需要根据实际需求确定

- 利用**树结构（递归）**解法
  ```java
  // definition of datatype
  datatype exp = Constant of int
               | Negate of exp
               | Add of exp * exp
               | Multiply of exp * exp
  
  // case function
  fun eval e = 
      case e of 
          Constant i => i
        | Negate e2  => ~ (eval e2)
        | Add(e1,e2) => (eval e1) + (eval e2)
        | Multiply(e1,e2) => (eval e1) * (eval e2)
        
  // usage
  eval (Add(Constant 19, Negate (Constant 4)))
  ```

### Type Synonyms
- datatype vs. type synonym
  datatype 独特于所有数据类型，只能通过constructor进行调用
  type synonym 只是某种数据类型的别名，REPL会把他们当作一样的
  ```java
  datatype suit = Club | Diamond | Heart | Spade
  datetype rank = Jack | Queen | King | Ace | Num of int
  
  type card = suit * rank
  type name_record = { student_num : int option,
                       first       : string,
                       middle      : string option,
                       last        : string }
                       
  fun is_Queen_of_Spades (c : card) =
      #1 c = Spade andalso #2 c = Queen
      
      // 以下三个既是 card 类型，又是 suit * rank 类型
  val c1 : card = (Diamond, Ace)
  val c2 : suit * rank = (Heart, Ace)
  val c3 = (Spade, Ace)        
  ```
  
### Lists and Options are Datatypes
- **Lists** 可以这么定义，[]确实是不带任何东西的constructor，::确实是带两个东西的constructor：
  ```java
  datatype my_int_list = Empty
                       | Cons of int * my_int_list
  val x = Cons(4, Cons(23, Cons(2008, Empty)))
  
  fun append_my_list (xs, ys) = 
      case xs of 
          Empty => ys
        | Cons(x, xs') => Cons(x, append_my_list(xs', ys)) // ML支持'用在变量名里
  ```
- **Options** 中 `NONE` and `Some` 其实是constructor而不是functions
  ```java
  fun inc_or_zero_intoption =
      case intoption of  
          NONE => 0      // 是不会发生重复的，所以顺序不重要
        | SOME i => i+1
  ```
- 事实上，像这样通过 Pattern-matching 的方法定义 List 和 Option 是更合理更安全的，之所以又会有`hd`,`valOf`存在是因为在传递参数的时候很有用，hw2不要用这些已经定义的constructor

### Polymorphic Datatypes
数据类型的多态性
  ```java
  datatype 'a option = NONE | SOME of a'
  datatype 'a mylist = Empty | Cons of a' * a' mylist
  datatype ('a, 'b) tree = 
         Node of 'a * ('a, 'b) tree * ('a, 'b) tree   // a' 就是该点值
       | Leaf of 'b
  ```

### Each-Of Pattern-Matching / Truth About Functions
1. 事实上所有bindings都是通过Pattern-Matching做到的
2. 事实上每个function都只接受一个参数

- 之前的学习Pattern-Maching都是**One-of**的，但是他也可以做到**Each-of**，比如去match **tuples** 或者 **record**
  ```java
  fun sum_triple triple =
      case triple of 
          (x, y, z) => x + y + z
  
  fun full_name r = 
      case r of 
          {first=x, middle=y, last=z} => 
              x ^ " " ^ y ^ " " ^ z
  ```
  - **Val Bindings** 其实也是在做 Pattern-Matching，比如当这么定义：`val NONE = SOME 2;`，会进入死循环(runtime exception)，因为找不到可以匹配的数据类型去对应SOME 和NONE
  - **Func bindings**：所有fun只有一个参数，那就Pattern，比如(int * int * int)就是一个参数（一种triple Pattern），可以引申出三种函数的定义方式：
  ```java
  fun sum_triple1 (triple : int * int * int) = 
      case triple of 
          (x, y, z) => x + y + z
  
  fun sum_triple2 triple =
      let val (x,y,z) = triple
      in 
          x + y + z
      end
  
  fun sum_triple3 (x,y,z) =    // 最简洁 
      x + y + z
  ```
  
### Type Inference
如果没有用到某个参数时，REPL会自动把那个参数当作 `a'`，即这个参数随便是什么都可以

### Polymorphic and Equality Types
- type 有 more general 这种说法，比如 `'a * int` 就比 `int * int` 更general，所以当函数参数为 `'a * int`时，利用多态性，就可以接收 `int * int` 或者 ` string * int`参数了
- **Equality Types**: 当函数内部出现等号的时候，编译器会自动检测类型是否需要定死，比如
  ```java
  // 没有定死所以类型是 'a * 'a -> string
  fun same_thing (x, y) =     
      if x=y then "yes" else "no"   
  
  //  定死了所以类型是 int -> string
  fun is_three x =
      if x=3 then "yes" else "no"
  ```

### Nested Patterns
有了nested pattern这个概念，if和else可以直接用pattern-matching替代了
- zip和unzip：zip是打包成 `(int * int * int)list`，unzip是拆成`int list * int list * int list`
  ```java
  fun zip3 list_triple = 
      case list_triple of 
          ([],[],[]) => []
        | (hd1::tl1,hd2::tl2,hd3::tl3) => (hd1,hd2,hd3)::zip3(tl1,tl2,tl3)
        | _ => raise ListLengthMismatch
  
  fun unzip3 lst = 
      case lst of
          [] => ([],[],[])
        | (a,b,c)::tl => let val (l1,l2,l3) = unzip3 tl
                         in
                             (a::l1, b::l2, c::l3)
                         end
  ```
- 非下降的list
  ```java
  fun nondecreasing xs = (* int list -> bool *)
      case xs of 
          [] => true  // 没有元素话肯定是非下降的
        | _::[] => true  // 只有1个元素的话肯定也是非下降的
        | head::(neck::rest) => head >= neck    // 2个及以上则需要判断了 
                                andalso nondecreasing (neck::rest)     
  ```
- 两数相乘后的符号
  ```java
  datatype sgn = P | N | Z
  fun multsign (x1, x2) = (* int * int -> sgn *)
      let fun sign x = if x=0 then Z else if x>0 then P else Z
      in 
          case (sign x1, sign x2) of
              (Z,_) => Z
            | (_,Z) => Z
            | (P,P) => P
            | (N,N) => P
            | _ => N     // 相当于otherwise
        //  但是下面这么写更保险，因为编译器会帮我们检查是否漏了哪种情况
        //  | (N,P) => N
        //  | (P,N) => N
  ```
- Nested Pattern可以使得代码更简洁优雅，避免使用 nested 的 case expression 和 不必要的分支或者let语句，善用通配符(wildcard)_

### Nested Patterns Precisely
过程：先确认 type 是否 match，然后再 bind 变量到一个值上去
- Pattern `a::b::c::d` matches all lists with >=3 elements
- Pattern `a::b::c::[]` mathces all lists with 3 elements
- Pattern `((a,b),(c,d))::e` matches all non-empty lists of pairs of pairs

### Optional: Function Patterns
一般函数定义：`fun f p = e`
之前用到 Nested Pattern 的时候，都是对 e 下工夫，进行分类，但是function pattern就像是函数重载，支持多种输入，直接对f p进行分类，如：
  ```java
  fun append ([],ys) = ys
    | append (x::xs', ys) = x :: append(xs', ys)
  ```
  但事实上，这个用case expression完全也可以表示的好，没有必要

### Exceptions
exception 的 数据类型是 exn, 也就是说可以作为参数传进函数，通过 raise 触发中断：
需要掌握：
  1. **exception binding**: `exception [Name]` 可以加上 `of int *int`表示同名exception也可以有不同的处理方式  
  2. **throw an exception**: `raise [Name]` 如果是of定义了exception的参数就需要赋上
  3. **catch an exception**: `e1 handle [Name] => e2`
  ```java
  // 定义自己的exception，当然也有已有的exception
  exception MyUndesirableCondition
  
  // 应用实例
  fun mydiv (x, y) =
      if y = 0
      then raise MyUndesirableCondition
      else x div y
      
  // exception作为参数传递
  fun maxlist (xs, es) = // int list * exn -> int
      case xs of
          []    => raise ex
        | x::[] => x
        | x::xs => Int.max(x, maxlist(xs', ex))
        
  // catch the exception，避免程序报错, x会被赋为42
  val x = maxlist ([], MyUndersirableCondition)
      handle MyUndesirableCondition => 42
  ```

### Tail Recursion
- Efficiency
  程序运行时，存在一个 **call stack** 记录所有开始了但是还没有返回的函数，rucursion 会导致 call stack中很多指针指向同一个函数
- **Tail Recursion**
  是提高recursion效率的一种优化方法，当递归获取的结果不需要额外的操作时，可以直接返回递归从底层(**tail position**)准备返回的值,最后一次递归（在tail position调用函数）也叫做 **tail call**:
  **Methodology**: 
  1. 创建一个包含 **accumulator** 的 helper 函数。accumulator暂存需要累加的值
  2. 上一步递归的结果变成新一次递归的初值
  3. 这一步递归的acc变成这一步的结果
  ```java
  // traditional recursion
  fun rev xs =
      case xs of
          [] => []
        | x::xs' => (rev xs) @ [x]    
    // 以上append操作在递归来看是 1+2+...+n次操作，所以时间复杂度是O(n^2)
    // 而以下的tail recursion是O(n)
  // tail recursion
  fun rev xs = 
      let fun aux(xs, acc) =    // helper func
              case xs of
                  [] => acc
                | x::xs' => aux(xs', x::acc)	
                // 在help func上不会做任何操作
                // 只改变 accumulator
      in
          aux(xs, [])
      end
  ```
- Complexity
  事实上，考虑空间复杂度，tail recursion 并不是万能药，比如处理树的时候
  
  

## Week 4: First-class functions
anonymous map and filter polymorphic

### Introduction to First-class Functions
先搞清楚 **Functional Programming** 函数式编程：
1. 大多数情况下避免 mutation 突变
2. 将函数当作值（本节内容）