Gradle plugins
==============

This is a collection of handy Gradle plugins I end up using over and over.


## integration-test

Adds integration testing to Gradle projects.

### How it works

* Requires previously applied Java plugin;
* Integration test sources must live under `src/integration-test/java`;
* Integration test resources must live under `src/integration-test/resources`.

### How to use

```groovy
apply from: 'https://raw.github.com/brunodecarvalho/gradle-plugins/integration-test.gradle'
```

To add to all subprojects of a multi-project build:

```groovy
subprojects {
    apply from: 'https://raw.github.com/brunodecarvalho/gradle-plugins/integration-test.gradle'
}
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
    apply from: 'https://raw.github.com/brunodecarvalho/gradle-plugins/jacoco-multiproject-aggregator.gradle'
}
```

The plugin must be applied to all subprojects. Here's what happens:

* If the project is a subproject, it introduces a pre-`test` hook to run JaCoCo agent and track coverage;
* If the project is the root project, a new task, `coverageReport`, is created.

The `coverageReport` task depends on `test` task so from a clean project, running `gradle coverageReport` will test the code and generate a coverage report.