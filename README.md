Gradle plugins
==============

This is a collection of handy Gradle plugins I end up using over and over.


## integration-test

Adds integration testing to Gradle projects, supporting Java, Groovy, Kotlin and Scala. It's completely dynamic, so it'll only add source directories for the source plugins you have (e.g. `java`, `resources`, `groovy` and `kotlin` but not `scala`).

### How it works

* It'll automatically pick directories under `src/integration-test/` that match `java`, `resources`, `groovy`, `kotlin` and `scala`, depending on whether you have any (or all) of those plugins enabled.

### How to use

```groovy
apply from: 'https://raw.github.com/brunodecarvalho/gradle-plugins/master/integration-test.gradle'
```

To add to all subprojects of a multi-project build:

```groovy
subprojects {
  apply from: 'https://raw.github.com/brunodecarvalho/gradle-plugins/master/integration-test.gradle'
}
```

#### ProTip™

If you're building projects with IntelliJ idea, you can automatically add the integration test sources by configuring the `module` model's test sources:

```groovy
idea {
  // ...
  module {
    testSourceDirs += file('src/integration-test/java')
  }
}
```

Or, if you're using a multi-project build:

```groovy
// On the root project's build.gradle
subprojects {
  // ...
  idea {
    module {
      testSourceDirs += file('src/integration-test/java')
      testSourceDirs += file('src/integration-test/groovy')
    }
  }
}
```


## colored-test-output

A picture worth a thousand words:

![colored test output sample](http://d.pr/i/8Jrg.png)

### How to use

```groovy
apply from: 'https://raw.github.com/brunodecarvalho/gradle-plugins/master/colored-test-output.gradle'
```


## jacoco-multiproject-aggregator

Adds coverage reports for multi-project Gradle projects.

### How it works

* Root project has no code;
* Coverage tracking is only done for submodules;
* A single coverage report is generated which aggregates all coverage data from subprojects.

### How to use

Apply on the root `build.gradle` project:

```groovy
allprojects {
  apply from: 'https://raw.github.com/brunodecarvalho/gradle-plugins/master/jacoco-multiproject-aggregator.gradle'
}
```

The plugin must be applied to all subprojects. Here's what happens:

* If the project is a subproject, it introduces a pre-`test` hook to run JaCoCo agent and track coverage;
* If the project is the root project, a new task, `coverageReport`, is created.

The `coverageReport` task depends on `test` task so from a clean project, running `gradle coverageReport` will test the code and generate a coverage report.

### License

MIT.
