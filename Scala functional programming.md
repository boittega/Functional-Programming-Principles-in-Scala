# Scala is a functional programing paradigm

Using Java or C, these languages use imperative programing, this paradigm has a problem when scaling up.
Functional programming, in the restricted sense, just means programming without mutable variables, assignments to those variables, loops and the other imperative control structures.
In the more general sense, functional programming means focusing on the functions in the program. It gives you new capabilities to work with these functions.
In particular, functions can be values that are produced, consumed and composed.

Functional programming focus on parallel while imperative focus on concurrency.
Scala is unified, agile, OO, functional, safe and performant, with strong static typing.

# evaluations, call-by-name and call-by-value

The evaluation of expressions is called substitution model.

Call-by-value has the advantage that every function argument is evaluated only once. 
Call-by-name has the advantage that a function argument is not evaluated at all if the corresponding parameter is not used in the evaluation of the function's body. 

When termination is not guaranteed, when have a infinite loop for example, if CBV evaluation of an expression terminates, then CBV evaluation will terminate too. The oposite is not true.

Scala normally uses call-by-value, but to uses call-by-name an arrow (=>) can be use, like:
```
def constOne(x: Int, y: => Int) = 1
```

# If else as an expression
If else as an expression is not a statement where you have to set a variable and the return a variable.
```
if (x >= 0) x else -x
```

The expressions can use:
- `!b` to negation
- `b && b` for AND
- `b || b` for OR

# definitions, by-name and by-value

The `def` is by-name, where the right hand side is evaluated on each use.

For by-value use val:
```
val x = 2
val y = square(x)
```

I this case `y` receives 4 and not square(2).

Another example is when defining a loop, the `def` will only refer the loop function, where `val` will enter in the infinite loop:
```
scala> def loop:Boolean = loop
loop: Boolean

scala> def x = loop
x: Boolean

scala> val x = loop
// [ctrl+c] to stop
```

A usage of this, is implementing an `and` function using by-name on the second parameter:
```
def and(x: Boolean, y: => Boolean) = if (x) y else false
```
This way if the `y` parameter receives a function like the `loop` function and `x` is `false`, the expression will return `false` and not enter in the infinte loop.

# Implementation of square root function using Newton's method

The function definiton:
```
def sqrt(x: Double): Double = ...
```
The Newton's method uses a initial estimate number `y`, we will use `1`, and repeatedly improve the estimate by taking the mean of `y` and `x/y`. This process will need a recursive function.
```
def sqrtIter(guess: Double, x: Double): Double =
    if (isGoogEnough(guess, x)) guess
    else sqrtIter(improve(guess, x), x)
```
For recursive functions in Scala the return type need to be explicit, otherwise it is optional. 

The result is:
```
def abs(x: Double) = if (x < 0) -x else x

def sqrtIter(guess: Double, x: Double): Double =
    if (isGoodEnough(guess, x)) guess
    else sqrtIter(improve(guess, x), x)

def isGoodEnough(guess: Double, x: Double) =
    abs(guess * guess - x) / x < 0.001

def improve(guess: Double, x: Double) =
    (guess + x / guess) / 2

def sqrt(x: Double) = sqrtIter(1.0, x)

println(sqrt(2))
println(sqrt(1e-6))
println(sqrt(1e60))
```

# Nested functions

Split a task in many small functions is a good functional programing syle, but to avoid the namespace polution and limit the access of the functions `isGoodEnough`, `impove` and `sqrtIter` to only the `sqrt` function and not to users, we can use nested functions.
```
def abs(x: Double) = if (x < 0) -x else x

    def sqrt(x: Double) = {
        
        def sqrtIter(guess: Double): Double =
            if (isGoodEnough(guess)) guess
            else sqrtIter(improve(guess))

        def isGoodEnough(guess: Double) =
            abs(guess * guess - x) / x < 0.001
        
        def improve(guess: Double) =
            (guess + x / guess) / 2
        
        sqrtIter(1.0)
    }
    println(sqrt(2))
    println(sqrt(1e-6))
    println(sqrt(1e60))
```
The functions `isGoodEnough`, `impove` and `sqrtIter` were mooved inside of the `sqrt` function definition using curly brakets (a block). Now those functions are only accesible for the function and not for the end user anymore. 
We also removed the duplication of `x` parameter on the definition of the iner functions, making the `sqrt` function much simpler.

# Multiple expressions in the same line and multi-line expressions

To join muiltiple expressions in the same line a semicolon (`;`) can be used:
```
val y = x + 1; y * y
```

To write a multi-line expression a plus (`+`) can be used in 2 different ways:
```
(oneExpression
+ otherExpression)
// or
oneExpression +
otherExpression
```

# Tail recursion
If a function calls itself or other function as its last action, the function's stack frame can be reused. Tail recursive functions are iterative processes, it is call tail-calls. For example:
```
def gcd(a: Int, b: Int): Int =
    if (b == 0) a else gcd(b, a % b)

factorial(14, 21)
```
the evaluation will be:
```
-> if (21 = 0) 14 else gcd(21, 14 % 21)
-> if (false) 14 else gcd(21, 14 % 21)
-> gcd(21, 14 % 21)
-> gcd(21, 14)
-> if (14 = 0) 21 else gcd(14, 21 % 14)
->> gcd(14, 7)
->> gcd(7, 0)
-> if (0 = 0) 7 else gcd(0, 7 % 0)
-> 7
```

Tail recursion as a special case of recursion that corresponds to a loop in an imperative program

# High order functions
It is a function that take other functions as parameters or return functions as results, the opposite of this is the first order function, that is the normal type which return simple data types.
It is a powerfull way to abstract and reuse functions, not always the best decision to keep the highest level of abstractions but it is important to know the techniques of abstraction and use them when apropriate.

# Anonymous functions
As functions are literal as a String, instead of defining functions we can define it anonymously, similar as a lambda function in Python. For example:

A normal function:
`def Cube(x: Int): Int = x * x * x`

An anonymous function:
`(x: Int) => x * x * x`

```
def sum(f: Int => Int, a: Int, b: Int) = {
  def loop(a: Int, acc: Int): Int =
  if (a > b) acc
  else loop(a + 1, f(a) + acc)
  loop(a, 0)
}
sum(x => x + x, 3, 5)
```

# Currying
Currying is when an anonymous function is nested in another anonymous function.

```
def sum2(f: Int => Int)(a: Int, b: Int): Int =
	if (a > b) 0 else f(a) + sum2(f)(a + 1, b)
sum2(x => x + x)(3, 5)
```
It combines two lists of parameters, what allow to call only the first if necessary:
```
sum2(x => x * x * x)
```

Here an example of currying as a way to define functions piecewise, one parameter section after the other.
In mathematics, a piecewise-defined function (also called a piecewise function or a hybrid function) is a function defined by multiple sub-functions, each sub-function applying to a certain interval of the main function's domain, a sub-domain.

```
def mapReduce(f: Int => Int, combine: (Int, Int) => Int, zero: Int)(a: Int, b: Int): Int = 
	if (a > b) zero
	else combine(f(a), mapReduce(f, combine, zero)(a + 1, b))

def product(f: Int => Int)(a: Int, b: Int): Int =
	mapReduce(f, (x, y) => x * y, 1)(a, b)
product(x => x * x)(3, 4)

def fact(n: Int) = product(x => x)(1, n)
fact(5)
```

# Solving sqrt function with currying

```
import math.abs
val tolerance = 0.0001
def isCloseEnough(x: Double, y: Double) =
    abs((x - y) / x) / x < tolerance
def fixedPoint(f: Double => Double)(firstGuess: Double) = {
    def iterate(guess: Double): Double = {
        val next = f(guess)
        if (isCloseEnough(guess, next)) next
        else iterate(next)
    }
    iterate(firstGuess)
}
def averageDump(f: Double => Double)(x: Double) = (x + f(x)) / 2

def sqrt(x: Double) = 
    fixedPoint(averageDump(y => x / y))(1)
sqrt(2)
```

# Scala Sytax
## notations
`|` denotes an alternative

`[...]` and option (0 or 1)

`{...}` a repetition (0 or more)

## Types
### Simple type 
SimpleType | FunctionType

### Function type
SimpleType `=>` Type | `(` [Types] `)` `=>` Type

Type can be Int, Double, Boolean, String, Function

## Expressions
identifier, like x, isGoodEnough

literal, like 0, 1.0, ”abc”

function application, like sqrt(x)

operator application, like -x, y + x

selection, like math.abs

conditional expression, like if (x < 0) -x else x

block, like { val x = math.abs(y) ; x * 2 }

anonymous function, like x => x + 1.

## Definitions
### Function definitions
def ident { `(` [ Parameters ] `)` }

[ `:` Type ] `=` Expr

### Value definitions
val ident [ `:` Type ] `=` Expr

### Parameters
ident `:` [ `=>` ] Type

where the => means by-name

# Compose and abstract data, create and encapsulate data structures
## Class and methods
Here I create a class called Rational where receives x and y  and add to parameters numer and denom. After I've created 4 methods, the first one will add 2 rationals, the third is to subtract 2 rationals using the add and the neg methods to add the negative version of the second rational. It is using the DRY principal, don't repeat yourself.
In the end I've created the toString method to be able to print the rationals better, for that is necessary to override the existing toString method.

```
class Rational(x: Int, y: Int){
  def numer = x
  def denom = y
  
  def add(that: Rational)  = 
  	new Rational(
      numer * that.denom + that.numer * denom,
      denom * that.denom)
  
  def neg: Rational = new Rational(-numer, denom)
  
  def sub(that: Rational) = add(that.neg)
  
  override def toString = numer + "/" + denom
}

val x = new Rational(1, 2)
val z = new Rational(5, 7)
val y = new Rational(3, 2)
x.denom
x.numer
x.add(y)
// result 2/1
x.sub(y).sub(z)
// result 12/-7
```

## Data abstraction
Now a gcd (greatest commom divisor) function was defined to simplify the rational. You can see it is a private function, which means it only  can be used inside of the class
Other change was divide the numer and denom by the result of gcd, doing that I've changed the numer and denom to val so the value is computed only once.
Here as well I've added less and max functions to calculate when one rational is smaller then 
```
class Rational(x: Int, y: Int){
  private def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
  val numer = x / gcd(x, y)
  val denom = y / gcd(x, y)
  
  def less(that: Rational) = numer * that.denom < that.numer * denom
  
  def max(that: Rational) = if (this.less(that)) that else this
  
  def add(that: Rational)  = 
  	new Rational(
      numer * that.denom + that.numer * denom,
      denom * that.denom)
  
  def neg: Rational = new Rational(-numer, denom)
  
  def sub(that: Rational) = add(that.neg)
  
	override def toString = numer + "/" + denom
}

val x = new Rational(1, 2)
val z = new Rational(5, 7)
val y = new Rational(3, 2)
y.add(y)
// result 3/1
x.less(y)
// result true
x.max(y)
// result 3/2
```

### require
If for example I create a rational with denominator 0 it will raise an error because it cannot be divided by zero.
To solve that I've added a requirement, or a precondition on a caller of a function, for my function using require, and it will raise an IllegalArgumentException error.
```
val strange = new Rational(1, 0)
strange.add(strange)
```
```
require(y != 0, "denominator must be nonzero") 
```
Another wai to do that is using assert, it will raise an AssertionError, assert is used to chech the code of the function itself.

## Constructors
The primary constructor simply takes the parameters of the class and executes all statements in the class body.
For a second contructor we can add this to the class:
```
def this(x: Int) = this(x, 1)
```
This way you have 2 ways to create a Rational object.

# Inflix Notation
It is possible in Scala to change a function `x.add(y)` to x + y, it is called inflix operator. for example:
```
def add(y: Int) = ...
```
can be changed to 
```
def + (y: Int)
```

For the neg function where you would like to use like `-x`, it is possible doing like:
```
def unary_- : Int = ...
```
Remember to leave a space after the minus signal.

# Class hierarchy
Using OO concepts of dynamic binding and family writing.

## Abstract classes

```
abstract class IntSet {
def incl(x: Int): IntSet
def contains(x: Int): Boolean
}
```

## AnyVal and AnyRef
There are 2 main scala type classes where most of other type classes inherit, the AnyVal that is the root for Int, Float, Double, Boolean, Char... and AnyRef that is the root of String, List, Seq, Iterable, ScalaObject and other java classes.
Those classe inherit from Any, this class define universal methods like `==`, `!=`, `equals`, `hashCode`, `toString`

## Conversion
In case 2 different types can be returned from a expression, if there is no direct conversion between them `AnyVal` is returned as type.
```
if (true) 1 else false
```
As between Boolean and Int there is no direct conversion, AnyVal is the returned type of this expression.
```
if (true) 1 else '0'
```
In this case Char and Int has direct conversion them Int is the returned type.

## Nothing 
Nothing is a class is a subtype of every other type and it is used when something terminates abnormally.
When using `throw` to raise an execption it will return Nothing.
```
def error(msg: String) = throw new Error(msg)
```

## Null
It is a class with value `null`.

# Scala is a Pure Object Oriented language


@@@ missing part

# Variance
how subtyping relates to genericity.

Contravariant uses a minus and a variant uses a plus:
```
trait Function[-T, +U] {
    def apply(x: T): U
}
```

functions are actually contravariant in their argument types, and covariant in their result types.
covariant type parameters only appear in method results.
Contravariant type parameters can only appear in method parameters, 
and nonvariant, or invariant, that they are used as aliases, type parameters can actually appear anywhere


# Decomposition
Let's say you have a hierarchy of classes and you want to build tree-like data structures from the instances of these classes.
How do you build such a tree? And how do you find out what kind of elements are in the tree? And how do you access the data that's stored in this elements? That's the problem of decomposition
Three attempts:
- Classification, that created a quadratic explosion of methods to write
- Type tests and type casts, unsafe at low level
- OO decomposition, doesn't work for all methods

# Pattern matching

## Case classes
A normal class but with `case` in front
```
trait Expr
case class Number(n: Int) extends Expr
case class Sum(e1: Expr, e2: Expr) extends Expr
```
Now when creating the eval method a case matching can be used to distinguish the class.
```
def eval(e: Expr): Int = e match {
    case Number(n) => n
    case Sum(e1, e2) => eval(e1) + eval(e2)
}
```

# List
List is imutable and array is mutable, and can change its content.
List are recursive, while arrays are flat.

Ways to construct a List.
The commom way
```
val fruit = List("apples", "oranges", "pears")
val nums = List(1, 2, 3, 4)
val diag3 = List(List(1, 0, 0), List(0, 1, 0), List(0, 0, 1))
val empty = List()
```
The List type way
```
val fruit: List[String] = List(ŏapplesŏ, ŏorangesŏ, ŏpearsŏ)
val nums : List[Int] = List(1, 2, 3, 4)
val diag3: List[List[Int]] = List(List(1, 0, 0), List(0, 1, 0), List(0, 0, 1))
val empty: List[Nothing] = List()
```
And using `cons` that is the `::`
```
fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
nums = 1 :: (2 :: (3 :: (4 :: Nil)))
empty = Nil
```
It can be simplified without the parenteses
```
nums = 1 :: 2 :: 3 :: 4 :: Nil
```
And as a method call
```
Nil.::(4).::(3).::(2).::(1)
```

## Sorting a list
One way to sort a list using insertion sorting.
```
def isort(xs: List[Int]): List[Int] = xs match {
    case List() => List()
    case y :: ys => insert(y, isort(ys))
}

def insert(x: Int, xs: List[Int]): List[Int] = xs match {
    case List() => List(x)
    case y :: ys => if (x <= y) x :: xs else y :: insert(x, ys)
}
```

