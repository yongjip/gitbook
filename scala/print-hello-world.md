---
description: print hello world
---

# Print "Hello world!"

src/main/scala/HelloWorld.scala

```scala
object HelloWorld {
  def main(args: Array[String]): Unit = {
    println("Hello, world!")
  }
}
```

Compile

```bash
scalac HelloWorld.scala
```

Run compiled file

```bash
scala HelloWorld
```
