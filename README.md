# Gradle ignoring import bug

This is an example project that tries to demonstrate a bug in gradle concerning imports of packages.

## Description

In Java, normally the name of a package should correspond to its physical location (i.e. package a.b.c maps to folders a/b/c).
Gradle seems to ignore this when the class is defined in a subproject.

In this example the class `Hello` should be in the package `com.example.hello` but its physical location is in the root folder `foobar` (see `src/main/java/foobar`).
The JVM correctly recognizes this as an error when you try to build the class via the command line:

```
Package name 'com.example.hello' does not correspond to the file path 'foobar'
```

But gradle happily builds this project into a jar and runs the main class:

```
$ ./gradlew run
16:28:29: Executing task 'run'...

> Task :hello:compileJava UP-TO-DATE
> Task :hello:processResources NO-SOURCE
> Task :hello:classes UP-TO-DATE

> Task :hello:run
Hello there!

BUILD SUCCESSFUL in 164ms
2 actionable tasks: 1 executed, 1 up-to-date
16:28:29: Task execution finished 'run'.
```

I tested this with JDK 8 and 11 with a gradle wrapper on version 6.2.1

## Minimal requirements to reproduce

I noticed this problem can only be reproduced when you're using subprojects.
If you move the src/ directory to the root project, then suddenly gradle will complain about the package name not corresponding to the physical location.

