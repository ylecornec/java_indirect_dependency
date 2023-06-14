# java_indirect_dependency
This reproduces a java compilation error which happens on macos with a nix provided bazel.

Running the following:
``` 
bazel build @@rules_jvm_external~5.1//private/tools/java/com/github/bazelbuild/rules_jvm_external/zip:zip
```
fails with:
```
external/rules_jvm_external~5.1/private/tools/java/com/github/bazelbuild/rules_jvm_external/zip/StableZipEntry.java:24: error: [strict] Using type java.util.zip.ZipEntry from an indirect dependency (TOOL_INFO: "/private/var/tmp/_bazel_runner/60c9feb4ce06ea417af25ad2f8083f7a/execroot/_main/bazel-out/darwin-fastbuild/bin/external/bazel_tools/tools/jdk/platformclasspath.jar").
public class StableZipEntry extends ZipEntry {
```

With the `--tool_java_language_version=11` setting, a remotejdk from rules_java is used instead of the local one (`@bazel_tools//tools/jdk:nonprebuilt_toolchain`) and the target compiles fine.

