_This material was written by [Aasmund Eldhuset](https://eldhuset.net/); it is owned by [Khan Academy](https://www.khanacademy.org/) and is licensed for use under [CC BY-NC-SA 3.0 US](https://creativecommons.org/licenses/by-nc-sa/3.0/us/). Please note that this is not a part of Khan Academy's official product offering._

---


## Packages

Every Kotlin file should belong to a _package_. This is somewhat similar to modules in Python, but files need to explicitly declare which package they belong to, and a package implicitly comes into existence whenever any file declares itself to belong to that package (as opposed to explicitly defining a module with `__init__.py` and having all the files in that directory implicitly belong to the module). The package declaration must go on the top of the file:

```kotlin
package content.exercises
```

If a file doesn't declare a package, it belongs to the nameless _default package_. This should be avoided, as it will make it hard to reference the symbols from that file in case of naming conflicts (you can't explicitly import the empty package).

Package names customarily correspond to the directory structure - note that the source file name should _not_ be a part of the package name (so if you follow this, file-level symbol names must be unique within an entire directory, not just within a file). However, this correspondence is not required, so if you're going to do interop with Java code and all your package names must start with the same prefix, e.g. `org.khanacademy`, you might be relieved to learn that you don't need to put all your code inside `org/khanacademy` (which is what Java would have forced you to do) - instead, you could start out with a directory called e.g. `content`, and the files inside it could declare that they belong to the package `org.khanacademy.content`. However, if you have a mixed project with both Kotlin and Java code, the convention is to use the Java-style package directories for Kotlin code too.

While the dots suggest that packages are nested inside each other, that's not actually the case from a language standpoint. While it's a good idea to organize your code such that the "subpackages" of `content`, such as  `content.exercises` and `content.articles`, both contain content-related code, these three packages are unrelated from a language standpoint. However, if you use _modules_ (as defined by your build system), it is typically the case that all "subpackages" go in the same module, in which case symbols with [`internal` visibility](visibility-modifiers.html) are visible throughout the subpackages.

Package names customarily contain only lowercase letters (no underscores) and the separating dots.


## Imports

In order to use something from a package, it is sufficient to use the package name to fully qualify the name of the symbol at the place where you use the symbol:

```kotlin
val exercise = content.exercises.Exercise()
```

This quickly gets unwieldy, so you will typically _import_ the symbols you need. You can import a specific symbol:

```kotlin
import content.exercises.Exercise
```

Or an entire package, which will bring in all the symbols from that package:

```kotlin
import content.exercises.*
```

With either version of the import, you can now simply do:

```kotlin
val exercise = Exercise()
```

If there is a naming conflict, you should usually import just one of the symbols and fully qualify the usages of the other. If both are heavily used, you can rename the symbol at import time:

```kotlin
import content.exercises.Exercise as Ex
```

In Kotlin, importing is a compile-time concept - importing something does not actually cause any code to run (unlike Python, where all top-level statements in a file are executed at import time). Therefore, circular imports are allowed, but they might suggest a design problem in your code. However, during execution, a class will be loaded the first time it (or any of its properties or functions) is referenced, and class loading causes [companion objects](objects-and-companion-objects.html#companion-objects) to be initialized - this can lead to runtime exceptions if you have circular dependencies.

Every file implicitly imports its own package and a number of built-in Kotlin and Java packages.




---

[← Previous: Functional programming](functional-programming.html) | [Next: Visibility modifiers →](visibility-modifiers.html)
