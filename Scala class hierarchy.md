# Class hierarchy

Classes and objects are organized in packages

```
package progfun.examples

object Hello {...}
```

```
> scala progfun.examples.Hello
```
To import a finction from a package, use `named imports`:
```
import week3.Rational
# or
import week3.{Rational, Hello}

object scratch {
    new Rational(1, 2)
}
```
to import everithing from a package use `wildcard import`:
```
import week3._ 
```
## Automatic Imports
All members of packages `scala`, `java.lang`, `scala.Predef` are automatically imported in any Scala program.
For example, Int, Boolean, Object, require, assert.

# Traits
One class can only have one superclass, called single inheritance.
A trait is declared like an abstract class:
```
trait Planar {
    def height: Int
    def width: Int
    def surface = height * width
}
```
Classes, objects and traits can inherit many traits, like:
```
class Square extends Shape with Planar with Movable ...
```
Traits cannot have (value) parameters but only fields and concrete methods.
