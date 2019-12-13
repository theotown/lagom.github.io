---
title: "Lagom 1.6 Released: Akka Persistence Typed, Projections Stop/Resume, Akka Jackson support and more"
date: 2019-12-13
author_github: renatocaval
tags: news
summary: >
    The Lagom team has released Lagom 1.6.0
---

We are happy to announce the availability of Lagom 1.6.0. This is the GA release of Lagom 1.6.0 and is production ready. We invite all our users to upgrade.

This version builds on top of the [2.8.x series of Play](https://blog.playframework.com/play-2-8-0-released/) and [Akka 2.6](https://akka.io/blog/news/2019/11/06/akka-2.6.0-released). See GitHub for the [full list of commits](https://github.com/lagom/lagom/compare/1.5.5...1.6.0).

Here’s a list of the most relevant improvements.

## Akka Typed and Akka Persistence Typed

Lagom 1.6 is based on the new Akka 2.6 release and have integrated support for Akka Typed.

As presented in the [announcement for Akka 2.6](https://www.lightbend.com/blog/six-things-architects-should-know-about-akka-2.6), the new Akka Actor APIs (known as Akka Typed) represent a major shift in the Akka ecosystem towards type-safety and more explicit guidance with Actors. We’re happy to bring this to all Lagom users as well.

Lagom includes dependency injection support for typed Actors in Akka 2.6 [through Play](https://www.playframework.com/documentation/2.8.x/AkkaTyped#Integrating-with-Akka-Typed).

While the Lagom Persistence API is still supported and maintained, the new Akka Persistence API ([Scala](https://www.lagomframework.com/documentation/1.6.x/scala/UsingAkkaPersistenceTyped.html)/[Java](https://www.lagomframework.com/documentation/1.6.x/java/UsingAkkaPersistenceTyped.html)) in Akka 2.6 is now the recommended default for persistence. This provides a more flexible API that gives you more control over some lower-level details, while retaining some of the guided approach that Lagom introduced. Akka Persistence can coexist with existing persistent entities, and the same read-side processor and topic producer APIs fully support both types of entities.

## Jackson serialization

For Java API, Lagom is now using the Akka Jackson serializer, which is an improved version of the serializer in Lagom 1.5. You can find more information about the Akka Jackson serializer in the [Akka documentation](https://doc.akka.io/docs/akka/2.6/serialization-jackson.html).

It is compatible with Lagom 1.5 in both directions. See our migration guide ([Scala](https://www.lagomframework.com/documentation/1.6.x/scala/Migration16.html)/[Java](https://www.lagomframework.com/documentation/1.6.x/java/Migration16.html)) for detailled information in case you are upgrading an existing Lagom application.

For Scala API, in addition to `play-json` support, Akka Jackson serializer is now also supported. Akka Jackson serialization is especially useful when using Akka Persistence Typed API as it allows serializaion of `ActorRef[T]` typically used when encoding command replies.

## Stop and Resume Projections

Lagom 1.6 has a new API to programmaticaly stop and resume projections ([Scala](https://www.lagomframework.com/documentation/1.6.x/scala/Projections.html)/[Java](https://www.lagomframework.com/documentation/1.6.x/java/Projections.html)) (Read Side Processors and Topic Producers) allowing users to control when a projection should start, stop or resume.

## Support for Scala 2.13 and Java 11

Lagom 1.6 supports the latest Scala version (2.13), LTS Java version (11) and sbt 1.3, as well as the earlier Scala 2.12 and Java 8 versions.

Scala 2.13 brings in a few preformance improvements for its collection and async (Future) APIs. Although Java users are not exposed directly to Scala APIs, the Lagom itself is built in Scala and uses many Scala dependencies. That said, updating Scala to v2.13 is also beneficial for Java API users.

## Migrating from previous versions

This release introduces very little changes over the `1.6.0-RC3` so if you already moved to that version the upgrade should be trivial. If you are still using Lagom `1.5.x` we recommend you first migrate to the latest version available of Lagom 1.5 series and then migrate to Lagom 1.6. If you are using a version older than `1.5.x`, you should migrate one version at a time. For example: from `1.4.9` to `1.4.15` (because `1.4.15` is `1.4.latest`), then from `1.4.15` to `1.5.latest` and finally to `1.6.x`. Read the appropriate [release notes](https://github.com/lagom/lagom/releases) and migration guides ([Java](https://www.lagomframework.com/documentation/1.6.x/java/Migration16.html) / [Scala](https://www.lagomframework.com/documentation/1.6.x/scala/Migration16.html)) on each step.

## Multiple changes in default behaviors

* Kafka client defaults to only 1 max inflight request per connection. This can mean a drop in throughtput but also increases guarantees against duplication or reordering of messages.
* persistence sharding uses ddata strategy instead of persistence
* JSON compression only triggers for payloads bigger than 32kb
* java.time.* serializes using ISO-8601instead of custom Jackson formats (only affects Java Lagom users)

## Credits

Thanks to the community for their detailed bug reports and contributions.

Thanks to [Lightbend](https://www.lightbend.com/) for their continued sponsorship of the Lagom core team’s efforts. Lightbend offers [commercial support](https://www.lightbend.com/subscription) for Lagom.

Special thanks to the following contributors who helped with this release:

```text
commits   added  removed  author
    249   25624    14553  Ignasi Marimon-Clos
    202   13607     7824  Renato Cavalcanti
    147    8846     3755  Marcos Pereira
     93    1704     2856  Dale Wijnand
     20     260      178  Tim Moore
     13     534      508  Patrik Nordwall
      6     200      155  Sergey Morgunov
      4      16       10  Alden Torres
      4      39       27  Enno Runne
      3      10        9  Martynas Mickevičius
      2       2        2  Abhiknoldur
      2     755      385  James Roper
      2      35       11  Ruth Stento
      1       3       46  Sethi, Kunal
      1       7        7  Juan Marin Otero
      1       7        7  Matthias Kurz
      1       2        2  Prashant Sharma
      1       0        0  0xflotus
      1       1        1  norfe
      1       0        6  Fabian Grutsch
      1       2        2  Fran Bermejo
      1       1        1  Johannes Rudolph
      1      24        1  Corey Auger
      1      10        0  Zhonglai Zhang
      1      72       60  Michael Liarakos
      1      27        3  Dmitriy Grigoryev
      1      23        1  Stephan Emmerich
      1       2        2  Steve Swing
```
