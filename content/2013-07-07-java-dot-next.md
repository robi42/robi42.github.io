+++
title = "Java.next?"
date = 2013-07-07
path = "blog/2013/07/07/java-dot-next"
[taxonomies]
#tags = ["Programming", "Code"]
+++

After several years of programming Java while always keeping an eye on alternatives around, I've recently come to the conclusion:

Actually, there are just a few things still missing in the language these days which would render me a *happy camper*&#8482; indeed (enjoying its mature ecosystem of libraries & tools).

Especially, since [Lombok](https://projectlombok.org/) takes a lot of the general boilerplate code PITA away here (with useful features like convenient property accessors, `hashCode/equals/toString` impl., logging facility injection, ...).

Also, [Guava](https://code.google.com/p/guava-libraries/wiki/GuavaExplained) does a truly good job in enabling one to write more concise and robust code.

Plus, Java 7 brought at least some nice, welcome [improvements](https://www.oracle.com/technetwork/java/javase/jdk7-relnotes-418459.html#changes) in<br>
&rarr; `try-with-resources`, exceptions `multicatch`, etc.

Finally, modern [DI](https://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection) with Spring 4 (or Guice) using `javax.inject.*` and Java config frees one from most needs for XML and, after all, a build sys beating Maven for real has yet to come.

So namely, here's a **personal wish list FTW**:

 * Pretty much everything [coming up with Java/JDK 8](https://www.techempower.com/blog/2013/03/26/everything-about-java-8/) (sooner than later, hopefully) -- particularly **lambdas** (at last...)
 * More **type inference** (which the JVM is totally capable of); Java 7's diamond operator's definitely a good start there but, please, let me write `val foos = ...` (like in Scala...)
 * **Collection literals** à la Python or C#, something like this would be really sweet:

```kotlin
     val foo = #{"bar": "baz"};
     foo["bar"] = "qux";
```

Pretty please. :)

PS: literal multiline strings, and so on and so forth, could be nice (but I can live without).<br>
PPS: wouldn't say no to optionally named function/constructor args, though.

**Update 2016:** these days, [Kotlin](https://kotlinlang.org/)'s the way to go, IMHO. :)
