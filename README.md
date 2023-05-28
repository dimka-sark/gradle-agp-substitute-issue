# gradle-agp-substitute-issue

## Overview

If application use dependency substitution it trigger AGP 

```groovy
configurations.all {
  resolutionStrategy.dependencySubstitution {
    substitute(module("lib-group:lib-name")).using(project(":localLib"))
  }
}
```

```
Configuration 'debugAnnotationProcessorClasspath' was resolved during configuration time.
This is a build performance and scalability issue.
```

## Step to reproduce bug

1. Open project
2. Select "app" project
3. Run `./gradlew :app:assembleDebug`
4. Compilation log performance issue

## Workaround solution

We can change substitution to exclude and implementation lib

```groovy
dependencies {
  implementation(project(":localLib"))
}

configurations.all {
  resolutionStrategy.dependencySubstitution {
    exclude("lib-group", "lib-name")
  }
}
```
