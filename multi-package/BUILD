load("@rules_java//java:defs.bzl", "java_binary")

package(default_visibility = ["//visibility:public"])

java_binary(
    name = "hello-world",
    srcs = ["src/main/java/com/cmr/HelloWorld.java"],
    main_class = "com.cmr.HelloWorld",
    deps = [":messenger-lib"],
)

java_library(
    name = "messenger-lib",
    srcs = ["src/main/java/com/cmr/messenger/Messenger.java"],
)