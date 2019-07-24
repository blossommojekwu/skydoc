licenses(["notice"])  # Apache 2.0

package(default_visibility = ["//visibility:private"])

exports_files([
    "templates/header.vm",
    "templates/func.vm",
    "templates/provider.vm",
    "templates/rule.vm",
])

load("//third_party/bazel_skylib:bzl_library.bzl", "bzl_library")
load("//third_party/py/skydoc/stardoc:stardoc.bzl", "stardoc")

filegroup(
    name = "test_deps",
    testonly = True,
    srcs = [
        "BUILD",
    ] + glob(["*.bzl"]),
    visibility = ["//visibility:public"],
)

bzl_library(
    name = "stardoc_lib",
    srcs = ["stardoc.bzl"],
    deps = [
        "//third_party/bazel_skylib:bzl_library",
    ],
)

stardoc(
    name = "stardoc_doc",
    out = "stardoc_doc.md",
    input = ":stardoc.bzl",
    symbol_names = [
        "stardoc",
        "_stardoc_impl",
    ],
    deps = [":stardoc_lib"],
)

java_binary(
    name = "stardoc",
    jvm_flags = [
        # quiet warnings from com.google.protobuf.UnsafeUtil,
        # see: https://github.com/google/protobuf/issues/3781
        # TODO(cparsons): Remove once Stardoc has the fix.
        "-XX:+IgnoreUnrecognizedVMOptions",
        "--add-opens=java.base/java.nio=ALL-UNNAMED",
        "--add-opens=java.base/java.lang=ALL-UNNAMED",
    ],
    main_class = "com.google.devtools.build.skydoc.SkydocMain",
    runtime_deps = [
        ":prebuilt_stardoc_binary",
    ],
)

java_import(
    name = "prebuilt_stardoc_binary",
    jars = ["stardoc_binary.jar"],
    visibility = ["//visibility:private"],
)

java_binary(
    name = "renderer",
    jvm_flags = [
        # quiet warnings from com.google.protobuf.UnsafeUtil,
        # see: https://github.com/google/protobuf/issues/3781
        # TODO(cparsons): Remove once Stardoc has the fix.
        "-XX:+IgnoreUnrecognizedVMOptions",
        "--add-opens=java.base/java.nio=ALL-UNNAMED",
        "--add-opens=java.base/java.lang=ALL-UNNAMED",
    ],
    main_class = "com.google.devtools.build.skydoc.renderer.RendererMain",
    runtime_deps = [
        ":prebuilt_renderer_binary",
    ],
)

java_import(
    name = "prebuilt_renderer_binary",
    jars = ["renderer_binary.jar"],
    visibility = ["//visibility:private"],
)
