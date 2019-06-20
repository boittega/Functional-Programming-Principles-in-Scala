# Create the build.sbt file

This file there are definitions of your project, add the name and scalaVersion into it:

```
name := "Hello World"

scalaVersion := "2.12.7"
```

# VS Code Metals import build

Import the build into Metals on VS Code and it will create the src folder for you.
Press F1 and search for `Import build`.

# Create some code

Inside src/main create a folder called scala and inside a file called helloWorld.scala.
Add the following code inside:

```
package helloWold

object helloWold extends App {
    println("Hello World")
}
```

# Compile and run the project 

Navigate to the project folder containing the build.sbt and start sbt, compile and run the project.

```
sbt
```

```
sbt:Hello World> compile
```

```
sbt:Hello World> run
```

I will print your first Hello World:

```
[info] Packaging /Users/rafaelbottega/Documents/scala/target/scala-2.12/hello-world_2.12-1.0.jar ...
[info] Done packaging.
[info] Running helloWold.helloWold 
Hello World
```