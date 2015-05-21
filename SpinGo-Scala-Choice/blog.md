# SpinGo <3 Scala

- Author: Tim Harper

![](img/scala.gif)

At SpinGo, we've been building our software on Scala, a statically-typed, multi-paradigm programming language for the JVM. We've enjoyed it and would like to share our experience.

# Why did we choose Scala?

Two years ago, SpinGo was running entirely PHP on the backend (using the framework CakePHP). The code was incredibly imperative, didn't have a single unit test, and the number of 500+ line methods were less rare than I'd like. As a company, we were playing whack-a-mole with our code; every time we'd change a feature, 3 or 4 things would break. This was an incredible drain to engineering morale: 50% of development effort was spent addressing regressions. We decided, as a company, that we would be better off in the long run picking a better set tools.

Some of the features we looked for in our next toolset were as follows:

- Functional programming support (specifically, higher-order functions, closures, lazy-evaluation and immutable data structures)
- Object-oriented like polymorphism support
- Strongly typed (and, specifically, a type system that doesn't get in the way)
- High signal, low noise
- Great community built around rich standard library
- Mature dependency management system
- Reasonable amount of inertia
- Open source
- Bonus: fast

![](img/functions.png)

We'd also felt it was time to switch our web-applications from server-side templating to a rich-client / API paradigm, so a language with great library support for writing JSON web-services was considered bonus, also.

We would've been willing to compromise on one feature in order to get really strong support in another. And, of course it bears mentioning that it is technically possible to write a Haskell compiler in PHP, so the argument of "any programmer worth the weight of his skin can write good code in any language" isn't helpful here. It's not about what you can do in your language, it's what that language makes easy, and the defaults that it assumes. We most deeply considered moving to `Ruby`, `Clojure`, `Go`, `Python`, `Haskell` or `C#`. All of these are fine choices, and companies have built great software in each of them. After researching and discovering `Scala`, it stood out as the obvious choice; it ranked very high on all of our wish-list items.

# Our experience with Scala

## Scala as a language

Overall, we've really enjoyed it. Scala is a tremendously rich language, with features including pattern-matching, monads and monadic operations built-in to the standard-library, for-comprehensions, multiple-inheritance via traits, type-inference, implicits, and many more! Personally, as one coming from predominantly Ruby and Clojure, implicits were one of the features that was most novel to me; I didn't appreciate it at first, but it's quickly become one of my favorite features of the language. The collection library is incredibly robust and contains most every collection manipulation method you need (although, no [transducers](http://clojure.org/transducers) here).

Some comments about experience with Scala from other members of our team:

> "After I spent a few hours studying Scala I decided I wanted to work in it full time.  A year after I couldn't imagine going back to writing Java. Scala changed the way I look at software problems." - [Jake Wilson](https://github.com/jakewilson801)

> "Transitioning from programming in Ruby to Scala has been great. Many of the features I enjoyed in Ruby are available in Scala but with a powerful type system, better functional programming support, and it's fast!" - [Joe Tanner](https://github.com/jtanner)

> "Having developed in Java for the past 15 years, I've found Scala to be a breath of fresh air. Elegant, concise, powerful, expressive. The power of functional programming is something you can't live without once you wrap your head around it. Your code becomes cleaner and more testable, and easier to reason about. And the designers and maintainers of Scala are as nice as they are smart, very willing to answer questions themselves and receive criticism as readily as they do praise. This keeps the language fresh, and progressing instead of stagnant." - [Eric Nelson](https://github.com/enelson)

## Ecosystem

[Typesafe](https://www.typesafe.com/) is the Open Source company behind [Akka](https://www.typesafe.com/community/core-projects/akka), [Spray](https://www.typesafe.com/blog/typesafe-gets-sprayed), [Slick](http://slick.typesafe.com), [Play](https://www.playframework.com), [Scala](http://www.scala-lang.org), and others; I am consistently impressed with the amount of thought put behind the entire platform. It's innovative, the products are a pleasure to use and they are really well designed. Further, our experience with Typesafe-backed products has been that they are incredibly stable. It's been rare that we've seen a major bug in even their Milestone releases.

The Scala maintainers are extremely active, friendly, and helpful. I had the pleasure of working with them in order to get a fix I'd made to Scala accepted (which, I'm proud to say landed in [2.11.5](http://www.scala-lang.org/news/2.11.5#contributors)! ).

## Tool support

Two years ago, when we began adopting Scala, tool support was a bit of a sore spot. Things have become drastically better since, with 3 solid options for Scala development. In order of our preference:

1. [Ensime / Emacs](https://github.com/ensime/ensime-server/wiki/Quick-Start-Guide) is our favorite Scala environment at SpinGo. It's a community effort and allows us to use Emacs with many of the features you'd expect from an IDE. My favorite thing about Ensime is that your code is indexed in an entirely separate process, so you can begin editing code immediately while Ensime boots up in the background (I think every IDE should be designed this way). As a bonus, Scala macros are handled properly.

2. Following that, [IntelliJ](https://www.jetbrains.com/idea/features/scala.html) would presently win second place. It works well, but you're forced to wait for the initial indexing process when booting up the editor. My biggest criticism of IntelliJ is it lacks proper Macro support, so you will encounter false negatives (code that IntelliJ thinks won't compile, but actually does). But, it performs well otherwise, and presently supports specific Scala macros used by more popular libraries.

3. [Scala IDE](http://scala-ide.org) is mature and very stable. It has full Macro support and it's code parsing is powered by code from the Scala compiler itself. The biggest downside is that it can, at times, become blocked on background tasks, rendering it slow to use. This has been getting better, and you can alleviate some of the performance issues by changing the default HEAP size for Eclipse, but as of this writing issues still exist.

### (Ensime sneak peak)

Here are some quick screenshots showing off Ensime in action:

#### Code completion using Ensime / Emacs:

![Ensime code-complete](img/ensime-complete.png)

#### Refactor tool support in Ensime / Emacs:

![Ensime refactor-rename](img/ensime-refactor.png)


## Complaints

No language or tool is perfect; Scala has some spots that occasionally cause us to feel sad. These are some lamentations:

- Type inference is complex, it can take a while to understand why in some cases it can infer the type properly, but in others it can't. As an upside, the set of rules for type inference are quite simple once you understand them, and they are applied consistently; it is unlikely that a beginning Scala programmer will fully grok this aspect of Scala.
- Compiler performance is still on the slow side. To be fair, the compiler is doing a LOT more work for you than other compilers (such as the one for Java). In practice, however, it's been manageable, and improved new incremental compiler that shipped with Scala 2.11 has reduced a lot this pain.
- Type erasure. This is a JVM limitation, and is fundamentally due to the decision of how Java implemented generics. In practice, it's a minor qualm, and is easy enough to work around
- Referencing abstract trait values inside of the trait constructor is a runtime exception waiting to happen. The trait constructor is run before the class which implements it; therefore, if the class initializes a value, then the value will not be available in the trait constructor. Fortunately, accessing the abstract values lazily is an elegant to the solution. The compiler should be able to detect this and warn about it, but presently doesn't.

Even with these considered, Scala is a tremendously pleasant environment in which to work. We remain optimistic about the future of Scala.

# Favorite libraries

We really, REALLY enjoy [Play-JSON](https://www.playframework.com/documentation/2.4.x/ScalaJson). Hands down, it is *the* single best JSON serialization library I have ever used. It's functional, it's ridiculously composable, and the way it uses Scala implicits is genius. The library uses no run-time reflection to load data into classes; instead, macros are used to generate readers and writers from your class structure at compile time. This enables concise format declaration with the option of breaking out and having as much control over serialization as you need. Additionally, since format dependencies are resolved at compile time, if you are missing a way to read a JSON string into a Joda DateTime object, you'll find out at compile time, not at run-time.

[Spray](http://spray.io) is also genius level. If you favor composition over inheritance (as we do at SpinGo), you'll likely enjoy it. Your logic for routing, handling headers, pulling data from the entity / query string, etc. can be concisely expressed using declarative, composable directives. It's incredibly fast and uses non-blocking IO. Note, Spray itself is being merged into the `Akka` project, as [Akka HTTP](http://doc.akka.io/docs/akka-stream-and-http-experimental/1.0-M2/scala/http/).

[Akka](http://doc.akka.io/docs/akka/2.3.11/scala.html?_ga=1.189539432.2145268044.1421160381) provides an excellent implementation of the actor model. While the actor pattern has it's limitations and can be easily abused, finite state machines are wonderful things and Akka supports this pattern marvelously.

[Akka Stream](http://doc.akka.io/docs/akka-stream-and-http-experimental/1.0-RC2/scala.html?_ga=1.189934440.2145268044.1421160381) is a Reactive Streams implementation and provides excellent support for functional stream processing. Reactive Streams is like RX (Reactive Extensions), but all stream components provide a signal for demand, enabling downstream components to slow down message producers in a non-blocking way. [Akka Stream] makes it easy to define various stream components in separate modules, and then connect them all together in a unified flow graph.

[Slick](http://slick.typesafe.com) is a functional relational database mapper. Queries are written using for-comprehensions, and can be validated by the Scala compiler at compile time. It is highly composable, too; making it easy to reuse partial query logic in a myriad of ways (and, the Scala compiler can assert these combinations will be valid).

[Op-Rabbit](https://github.com/SpinGo/op-rabbit); okay, shameless plug here. We developed `Op-Rabbit`. It's a high-level, opinionated RabbitMQ toolkit for implementing common message patterns. You should check it out.

# Scala helped us reach our technology goals

Scala has been a tremendous force for good in helping us to meet our technology goals. Scala's support for functional programming has made it an absolute joy to program in that paradigm. The `Option` monad alone has entirely obliterated null exceptions in our code ([DEATH TO NULL!!!](https://github.com/ensime/ensime-emacs/blob/146e9adc570fd78560ef93a1d2c63a590ccfe1ab/ensime-client.el#L801)), since we are forced to express, using the type-system, when a value is or isn't required. Our regression rate is tremendously low, and our various API servers have enjoyed a 99.9%+ uptime, month over month. This means that scrambling to put out fires is the exception, not the norm, and we're more able to focus on building new software and doing so without a constant sense of urgency. As mentioned before, the concurrency, serialization and HTTP libraries are also an absolute joy to use.

# Conclusion

If you're asking whether you should consider learning and using Scala, I think the answer is yes. Scala has been proven over and over ([by Twitter, Apple, LinkedIn, Netflix, Foursquare, etc.](http://alvinalexander.com/scala/whos-using-scala-akka-play-framework)) to be a great technology choice. It's backed by a successful, innovative, open source company, [Typesafe](https://www.typesafe.com), which also develops and offers paid support for various prominent projects in the Scala community, including [Apache Spark](https://www.typesafe.com/community/other-projects/apache-spark). It's feature rich, has a vibrant community, and in our experience the code that we produce using Scala generally works on the first run.

So, yes, you should definitely consider it. And, right now is a great time to adopt it. Tool support has generally arrived, and the language and ecosystem are very mature.

