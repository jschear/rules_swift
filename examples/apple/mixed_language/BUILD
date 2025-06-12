load("//mixed_language:header_map.bzl", "header_map")
load("//mixed_language:mixed_language_library.bzl", "mixed_language_library")
load("//swift:swift_library.bzl", "swift_library")
load("//swift:swift_test.bzl", "swift_test")

header_map(
    name = "swift_headermap",
    module_name = "MixedAnswer",
    propagate_include = False,
    # This relies on us knowing the naming scheme used for internal targets in mixed_language_library, which is not ideal.
    deps = ["MixedAnswer_swift"],
)

# Using `deps` doesn't work, as it leads to a dependency cycle:
#
# ~/Development/rules_swift js/mixed_language_module_clang_only_deps *+ $ bazel build //examples/apple/mixed_language:MixedAnswer
# ERROR: /Users/jschear/Development/rules_swift/examples/apple/mixed_language/BUILD:13:23: in swift_library rule //examples/apple/mixed_language:MixedAnswer_swift: cycle in dependency graph:
#     //examples/apple/mixed_language:MixedAnswer (0197940c625eb0199ad1b251a2ce1edfe467314f8905f8cbb11b2dbb55d11ff5)
#     //examples/apple/mixed_language:MixedAnswer (255e5a12d821f8f670762b1f5226e7bbc337500a6e2ac1a238850a1d16c2e75a)
# .-> //examples/apple/mixed_language:MixedAnswer_swift (255e5a12d821f8f670762b1f5226e7bbc337500a6e2ac1a238850a1d16c2e75a)
# |   //examples/apple/mixed_language:swift_headermap (255e5a12d821f8f670762b1f5226e7bbc337500a6e2ac1a238850a1d16c2e75a)
# `-- //examples/apple/mixed_language:MixedAnswer_swift (255e5a12d821f8f670762b1f5226e7bbc337500a6e2ac1a238850a1d16c2e75a)
# ERROR: Analysis of target '//examples/apple/mixed_language:MixedAnswer' failed; build aborted
# mixed_language_library(
#     name = "MixedAnswer",
#     hdrs = ["MixedAnswer.h"],
#     clang_srcs = [
#         "MixedAnswer.m",
#         "MixedAnswerPrivate.m",
#         "MixedAnswerPrivate.h",
#     ],
#     enable_modules = True,
#     module_name = "MixedAnswer",
#     swift_srcs = [
#         "MixedAnswer.swift",
#     ],
#     target_compatible_with = ["@platforms//os:macos"],
#     deps = [":swift_headermap"],
# )

mixed_language_library(
    name = "MixedAnswer",
    hdrs = ["MixedAnswer.h"],
    clang_copts = [
        "-I$(execpath :swift_headermap)",
        "-I.",
    ],
    clang_only_deps = [":swift_headermap"],
    clang_srcs = [
        "MixedAnswer.m",
        "MixedAnswerPrivate.m",
        "MixedAnswerPrivate.h",
    ],
    enable_modules = True,
    module_name = "MixedAnswer",
    swift_srcs = [
        "MixedAnswer.swift",
    ],
    target_compatible_with = ["@platforms//os:macos"],
)

swift_library(
    name = "SwiftLibDependingOnMixedLib",
    srcs = [
        "SwiftLibDependingOnMixedLib.swift",
    ],
    module_name = "SwiftLibDependingOnMixedLib",
    deps = [":MixedAnswer"],
)

mixed_language_library(
    name = "MixedTestsLib",
    testonly = True,
    clang_srcs = [
        "MixedTests.m",
    ],
    swift_srcs = [
        "MixedTests.swift",
    ],
    target_compatible_with = ["@platforms//os:macos"],
    deps = [
        ":SwiftLibDependingOnMixedLib",
    ],
)

swift_test(
    name = "MixedTests",
    deps = [":MixedTestsLib"],
)
