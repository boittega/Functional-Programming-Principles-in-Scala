# List of commands on sbt console

after create a scala project and start sbt:

```
sbt
```

The sbt and project name appears:

```
sbt:Hello World>
```

## Compile

Run `compile` to compile the project located in the directory src/main/scala.

```
sbt:Hello World> compile
[info] Updating ...
[info] Done updating.
[info] Compiling 1 Scala source to /Users/rafaelbottega/Documents/scala/target/scala-2.12/classes ...
[info] Non-compiled module 'compiler-bridge_2.12' for Scala 2.12.7. Compiling...
[info]   Compilation completed in 7.36s.
[info] Done compiling.
[success] Total time: 9 s, completed 24-May-2019 10:56:35
```

## Run

Run `run` to run the project.

```
sbt:Hello World> run
[info] Packaging /Users/rafaelbottega/Documents/scala/target/scala-2.12/hello-world_2.12-1.0.jar ...
[info] Done packaging.
[info] Running helloWold.helloWold 
Hello World
```

## Console

Run `console` to open Scala interpreter inside sbt. Press [ctrl-d] to exit

```
sbt:Hello World> console
[info] Starting scala interpreter...
Welcome to Scala 2.12.7 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_171).
Type in expressions for evaluation. Or try :help.

scala> println("hi!")
hi!

scala> val l = List(1, 2, 3)
l: List[Int] = List(1, 2, 3)

scala> val squares = l.map(x => x * x)
squares: List[Int] = List(1, 4, 9)

scala> 
[success] Total time: 80 s, completed 24-May-2019 11:20:02
```

## Test

Run `test` to test the project using the tests located in the directory src/test/scala.

```
sbt:Hello World> test
[info] Run completed in 35 milliseconds.
[info] Total number of tests run: 0
[info] Suites: completed 0, aborted 0
[info] Tests: succeeded 0, failed 0, canceled 0, ignored 0, pending 0
[info] No tests were executed.
[success] Total time: 1 s, completed 24-May-2019 11:20:19
```
