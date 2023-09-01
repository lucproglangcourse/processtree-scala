[![Scala CI](https://github.com/lucproglangcourse/processtree-scala/actions/workflows/scala.yml/badge.svg)](https://github.com/lucproglangcourse/processtree-scala/actions/workflows/scala.yml)
[![codecov](https://codecov.io/gh/LoyolaChicagoCode/processtree-scala/branch/master/graph/badge.svg)](https://codecov.io/gh/LoyolaChicagoCode/processtree-scala)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/e1a75747962b4c45aef938df10e3e1da)](https://www.codacy.com/gh/LoyolaChicagoCode/processtree-scala/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=LoyolaChicagoCode/processtree-scala&amp;utm_campaign=Badge_Grade)
[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org)

[![Average time to resolve an issue](http://isitmaintained.com/badge/resolution/LoyolaChicagoCode/processtree-scala.svg)](http://isitmaintained.com/project/LoyolaChicagoCode/processtree-scala "Average time to resolve an issue")
[![Percentage of issues still open](http://isitmaintained.com/badge/open/LoyolaChicagoCode/processtree-scala.svg)](http://isitmaintained.com/project/LoyolaChicagoCode/processtree-scala "Percentage of issues still open")

# Learning Objectives

- exploration of a standard problem in Scala:
  edge reversal algorithm:
  converting a representation of a tree as a flat sequence of child-to-parent
  edges to one where each parent maps to a sequence of children
  - OO/imperative, with mutable data structures
  - functional, with immutable data structures
- software design principles
  - [SoC](https://en.wikipedia.org/wiki/Separation_of_concerns) (separation of concerns)
  - [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (don't repeat yourself)
  - [testcase superclass](http://xunitpatterns.com/Testcase%20Superclass.html)
- Scala programming techniques
  - console input
  - [stackable traits](http://www.artima.com/scalazine/articles/stackable_trait_pattern.html)
  - unit testing using [ScalaTest](http://www.scalatest.org)
  - automated performance testing/microbenchmarking using [ScalaMeter](https://scalameter.github.io)

# Overview

This is a Scala-based solution to the
[process tree homework assignment](http://osdi.etl.luc.edu/homework/home-lab-assignment-1)
from the course
[COMP 374/410: Introduction to Operating
Systems](http://osdi.etl.luc.edu).

In short, this program converts a flat list of current processes, as printed by the `ps` command

```
> ps -ef
  UID   PID  PPID   C STIME   TTY           TIME CMD
    0     1     0   0 24Aug23 ??        72:25.36 /sbin/launchd
    0   102     1   0 24Aug23 ??        26:34.38 /usr/libexec/logd
    0   103     1   0 24Aug23 ??         0:01.08 /usr/libexec/smd
...
  502 60706 60704   0 10:53PM ??         0:02.27 /Applications/WhatsApp.app/...
...
  502 65138   735   0 10:48AM ??         0:23.83 /Applications/Microsoft Edge.app/...
...
```

to a hierarchical process tree, as printed by the `pstree` command (you'd usually have to install this through your package manager).

```
> pstree
-+= 00001 root /sbin/launchd
 |--= 00102 root /usr/libexec/logd
 |--= 00103 root /usr/libexec/smd
 .
 .
 .
 |-+= 00735 laufer /Applications/Microsoft Edge.app/Contents/MacOS/Microsoft Edge
 | |--- 65138 laufer /Applications/Microsoft Edge.app/Contents/Frameworks/Microsoft Edge
 .
 .
 .
 |-+= 60704 laufer /Applications/WhatsApp.app/...
 | |--- 60706 laufer /Applications/WhatsApp.app/...
 .
 .
 .
```


# Usage

To run the tests:

```
$ sbt test
```

To run the main methods:

```
$ ps -ef | sbt "runMain edu.luc.etl.osdi.processtree.scala.mutable.Main"
$ ps -ef | sbt "runMain edu.luc.etl.osdi.processtree.scala.groupby.Main"
$ ps -ef | sbt "runMain edu.luc.etl.osdi.processtree.scala.fold.Main"
```

To generate larger data sets for testing:

```
$ sbt "test:runMain edu.luc.etl.osdi.processtree.scala.fakeps.Main 100000" > data.txt
```

To run the benchmarks:

```
$ sbt "test:runMain edu.luc.etl.osdi.processtree.scala.fakeps.Benchmark"
$ sbt "test:runMain edu.luc.etl.osdi.processtree.scala.mutable.Benchmark"
$ sbt "test:runMain edu.luc.etl.osdi.processtree.scala.groupby.Benchmark"
$ sbt "test:runMain edu.luc.etl.osdi.processtree.scala.fold.Benchmark"
```

On Windows, if you installed [Git](http://git-scm.com/) with the recommended
third option, *Use Git and optional Unix tools from the Windows Command Prompt*,
then you will have a `ps` command available.
