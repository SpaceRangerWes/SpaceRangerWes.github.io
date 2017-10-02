---
layout: post
title: Dependency Management With Maven
---

Maven is a build automation tool that defines how a Java project should be built and the dependencies on which the project relies. If you would like to learn about Maven, I highly recommend their tutorial [What is Maven?](https://maven.apache.org/what-is-maven.html). With that said, this is not a post about the beginning tutorials of using Maven. I'm more interested in the organization of project dependencies in large enterprise Java projects.

When I first started at Cerner, I wanted to learn about data science, distributed computing, Hadoop, Apache Spark, and ad-hoc, real-time big data analytics. Over the course of five months, I've somehow ended up taking care of dependency trees. It all started when we wanted to upgrade our version of the Cloudera Hadoop Distribution from 5.4.4 to 5.5.2. If you are anything like me, you're thinking "_Oh, I'll have this out in an iteration easy. What could go wrong with a 0.1 version upgrade?!_"

> Many things, my pained friend.

Over the course of uplifting half of the Hadoop cluster to CDH5 from CDH4, my team (Enterprise Analytics) ran into quite a few road blocks. These appeared to be only configuration level deep. Even so, debugging and developing Apache Oozie workflow configs, changing maven dependency versions to play nice with the runtime environment, and trimming up dependency conflicts takes a lot of time... Weeks... Months...

So let's start with some quick definitions.

## Transitive Dependencies ##

Starting in Maven 2.0, it was decided that,

> ...to allow you to avoid the need of discovering and specifying the libraries that your own dependencies require...

you can use transitive dependencies.

A **transitive dependency** is a library which may be included or used in a Java package that is inherited from the project's parents, the parents' dependencies, and so on.

Because of this, you may imagine a situation where you've included five different minor versions of Hadoop (or any other library) in the graph of included libraries.

>_Which version will it use, and is it the same as what we have on the cluster?!_

> Beats me

Because of this new transitive dependency feature, Maven introduced additional features that helps you limit your dependency trees.

* Dependency Mediation
* **Dependency Management**
* Dependency Scope
* Excluded Dependencies
* Optional Dependencies

I'm looking to cover Dependency Management. Perhaps I'll cover the others at a later date.

Before we cover **Dependency Management**, we need to understand...

## Dependency Scope ##

> Dependency scope is used to limit the transitivity of a dependency, and also to affect the [classpath](https://en.wikipedia.org/wiki/Classpath) used for various build tasks.

We have 6 scopes available for use:

* **compile**

  If no scope is specified, this is the default value. If a dependency is given the `compile` scope, it is available in all classpaths of your project.

  **_Important_**: These depedencies are propagated to dependent projects. This means that if project B depends on project A and project A has a `compile` scoped dependency, B will inherit this dependency.

* **provided**

  Similar to `compile`, but it indicates you expect the dependency to be provided at runtime by the environment. A `provided` dependency is not transitive.

* **runtime**

  This scope indicates that we do not need the dependency for a successful compilation, but it is needed at runtime for code execution. The dependency will not be in the `compile` classpath.

* **test**

  This one is a little more obvious. You only need the dependency for testing purposes.

* **system**

  Be very careful with the `system` scope. Essentially, we are saying "_I will provide the path to the jar explicitly and not yours (usually the runtime environment)_." Avoid this because you can not guarentee that each system the project runs on is the same. This can cause strange, unforeseen errors.

* **import** (Maven 2.0.9^)

  This is only used with a `pom` type depedency in a `<dependencyManagement>` section. This will be important for us later, and I'll cover it in more detail.

## Dependency Management ##

A `<dependencyManagement>` section is used for centralizing dependency information. This is used to put all qualities of a dependency in the common POM (usually a parent POM) to have simpler references to the artifacts (the specific class of a group; i.e. `hadoop-core` or `jersey-servlet`). In the Population Health Organization at Cerner, we have many teams that work at different layers of the data processing pipeline, but we all use the same infrastructure. I'm speaking of our Hadoop clusters, load balancers, single-sign-on services, Chef and Rails utility nodes (servers), etc.

* Look at Maven's [Introduction to the Dependency Mechanism](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Management) for a good example.

Because we have over a dozen teams and hundreds of projects across those teams (while often times ingesting each others workflows, data, and code), Cerner's Pop Health Org has a **dependency management POM** specifically for keeping the teams and projects in sync. This way we can all use similar dependency versions, and also know that our code is going to be compatible with our distributed clusters.

If you have messed with POMs and POM inheritance before, you may be asking "_If `dependencyManagement` is defined in a parent POM and used by its children just like a `dependency` is, what makes it any different than a `dependency`_?"

Artifacts specified in the `<dependencies>` section are **ALWAYS** included as a dependency of the child project. Artifacts specified in the `<dependencyManagement>` section of a parent POM is only included in its children if they specify that dependency. Now what is the benefit in that? Well... you now aren't forced in inheriting code you don't want. Your packaged jars now contain less overhead when you don't use a dependency from a parent project. But using `<dependencyManagement>` has some caveats.

### Using `<dependencyManagement>` ###

Lets use a real world example. This is an excerpt from an anonymous dependency management POM from our infrastructures team that our Org used to use.

>Sorry there be no secret hurr!

###### Infra's Dependency Management ######
```xml
.
.
.
<dependencyManagement>
  <dependencies>
    <!--  Spark  -->
    <dependency>
      <groupId>org.apache.crunch</groupId>
      <artifactId>crunch-spark</artifactId>
      <version>${crunchVersion}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.spark</groupId>
      <artifactId>spark-core_2.10</artifactId>
      <version>${sparkVersion}</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.spark</groupId>
      <artifactId>spark-streaming_2.10</artifactId>
      <version>${sparkVersion}</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.spark</groupId>
      <artifactId>spark-yarn_2.10</artifactId>
      <version>${sparkVersion}</version>
      <scope>provided</scope>
    </dependency>
    .
    .
    .
  </dependencies>
</dependencyManagement>
.
.
.
```

###### Example Usage of `<dependencyManagement>` in a Downstream Project ######
```xml
.
.
.
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>com.example.infrastructure</groupId>
      <artifactId>hadoop-dependency-mgmt-cdh5</artifactId>
      <version>1.0</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  .
  .
  .
  </dependencies>
</dependencyManagement>

<dependencies>
  <dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-core_2.10</artifactId>
  </dependency>
</dependencies>
.
.
.
```

Now hopefully there are two things that stood out to you.

1. We used the `import` scope to import infra's dependency management POM.
2. We did not define scope or version.

###### A note on `import` ######
A POM can only inherit from a single parent POM. In large projects, this may not be acceptable. This is the purpose of declaring a `<dependencyManagement>` section with the `import` scope. It is an effective mechanism for defining a library of related artifacts. For example, you may want a library of dependencies relating to Google's Guice dependency injection project. You can now stay in sync across multiple projects utilizing Guice and its comrades.

###### A note on the Lack of Defining Dependency Info ######
You may be asking, "_Why didn't we define the scope and version?_" This is because the `<dependencyManagement>` is trying to lighten your load. Remember this is all about staying in sync across multiple projects. My team has over 24 projects alone, and it is very challenging trying to keep their dependency versions in sync.

**_Important_**: Do not specify a dependencies scope, version, or exclusions in the consuming POM. This will overwrite the transitive dependency and now use the version and specifics which you have defined locally. You are now out of sync with the other projects ingesting your dependency manager.

---

If I have missed any important information, or you would like me to go into any greater details, email a request to spacerangerwes@gmail.com. I will try to see if I can answer your question with the addition of content to this post.

Check out [Maven Best Practices](http://www.kyleblaney.com/maven-best-practices/) by Kyle Blaney. It's a good quick reference.

---

## References ##
* https://maven.apache.org/guides/

* http://stackoverflow.com/questions/2619598/differences-between-dependencymanagement-and-dependencies-in-maven?noredirect=1&lq=1

* https://en.wikipedia.org/wiki/Classpath_(Java)

* http://www.kyleblaney.com/maven-best-practices/
