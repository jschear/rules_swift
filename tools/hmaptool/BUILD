load(
    "//swift:swift_binary.bzl",
    "swift_binary",
)
load(
    "//swift:swift_library.bzl",
    "swift_library",
)

swift_library(
    name = "BinaryHeaderMapTool",
    srcs = [
        "BinaryHeaderMap.swift",
        "BinaryHeaderMapEncoder.swift",
        "BinaryHeaderMapTool.swift",
    ],
)

swift_binary(
    name = "hmaptool",
    # Used by the rule implementations, so it needs to be public; but
    # should be considered an implementation detail of the rules and
    # not used by other things.
    visibility = ["//visibility:public"],
    deps = [":BinaryHeaderMapTool"],
)
