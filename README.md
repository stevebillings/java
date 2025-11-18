## Java on Spring Boot

My primary language for professional use for about 15 years was Java, and most of that work also used Spring Boot. 

Most of the code I worked on while at Black Duck Software was written in Java on Spring Boot, and much of it is open source in github. Here are some examples of that work.

### New code

For a look at some code for which I was the primary developer:

1. git clone https://github.com/blackducksoftware/detect.git
1. cd detect
1. git checkout 8.0.0
1. cd detectable/src/main/java/com/blackduck/integration/detectable/detectables/bazel/

The bazel package contains the Black Duck Detect code that provided support for Bazel projects (see [Bazel support](https://documentation.blackduck.com/bundle/detect/page/packagemgrs/bazel.html). While everything we did was a team effort, I was responsible for the design, developed the initial version of it, and, as of Detect 8.0.0, had done the majority of the work on this code.

#### Background

The Bazel detector’s job is to discover dependencies for any of several software project types that use the [Bazel](https://bazel.build/) build tool. Depending on the project type, the detector would run a sequence of steps that included running Bazel commands, parsing the output, using elements of that output as arguments in subsequent Bazel commands, etc. This sequence of steps eventually results in a graph representing the project’s dependencies, that Detect would feed into the [Black Duck SCA system](https://www.blackduck.com/software-composition-analysis-tools/black-duck-sca.html) via Black Duck's REST APIs. The Bazel detector design is based on a set of “pipelines” (think unix pipes; see class Pipelines). It has one pipeline per project type. Each pipeline combines a set of general-purpose steps (see the classes in package bazel.pipeline.step) in a sequence. Examples of steps include: execute command, filter, split, de-dup, replace, parse, etc. This approach greatly reduced the amount of code required to support all of the required project types, and greatly reduced the incremental effort required to add support for a new project type.

### Enhanced code

This [pull request](https://github.com/blackducksoftware/detect/pull/516) shows a pretty typical change to a detector (in this case: the [BitBake Detector](https://documentation.blackduck.com/bundle/detect/page/packagemgrs/bitbake.html)). It includes new code (and tests) that I wrote, changes I made to existing code, and team interaction during code review.

#### Background

The BitBake Detector discovers dependencies in [BitBake](https://docs.yoctoproject.org/1.6/bitbake-user-manual/bitbake-user-manual.html) projects.
It does this by executing a BitBake command, parsing the output, executing another BitBake command using information parsed from the output of the previous command, etc. This sequence of steps eventually results in a graph representing the project’s dependencies, that Detect would feed into the [Black Duck SCA system](https://www.blackduck.com/software-composition-analysis-tools/black-duck-sca.html) via Black Duck's REST APIs. Unlike the Bazel detector, the BitBake detector only supports a single project type, so has no need for a pipeline approach.
