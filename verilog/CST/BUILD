# This package contains utility functions that extract or operate on
# the concrete syntax tree (cst) structure built up by verilog.y.
# Any code that relies on the internal tree structure should be isolated in
# here.

licenses(["notice"])

package(
    default_visibility = [
        "//verilog/analysis:__subpackages__",
        "//verilog/formatting:__subpackages__",
        "//verilog/parser:__subpackages__",
        "//verilog/tools/syntax:__pkg__",  # for printing
    ],
)

# Generate foreach list for nonterminal enums.
# Libraries that need these generated include files should list them
# under srcs, even if they are included by headers.
genrule(
    name = "verilog_nonterminals_foreach_gen",
    srcs = ["verilog_nonterminals.h"],
    outs = ["verilog_nonterminals_foreach-gen.inc"],
    cmd = "sed -n -e '/BEGIN GENERATE/,/END GENERATE/p' $< | " +
          "grep -v GENERATE | " +
          "sed -e 's|^ *|CONSIDER(|' -e 's| =.*,|,|' -e 's|,|)|' > $@",
)

cc_library(
    name = "verilog_nonterminals",
    srcs = [
        "verilog_nonterminals.cc",
        "verilog_nonterminals_foreach.inc",
        "verilog_nonterminals_foreach-gen.inc",
    ],
    hdrs = [
        "verilog_nonterminals.h",
    ],
    deps = [
        "//common/text:constants",
        "@com_google_absl//absl/strings",
    ],
)

cc_test(
    name = "verilog_nonterminals_test",
    srcs = ["verilog_nonterminals_test.cc"],
    deps = [
        ":verilog_nonterminals",
        "//common/text:constants",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "verilog_matchers",
    srcs = [
        "verilog_matchers.cc",
        "verilog_nonterminals_foreach.inc",
        "verilog_nonterminals_foreach-gen.inc",
    ],
    hdrs = [
        "verilog_matchers.h",
    ],
    deps = [
        ":verilog_nonterminals",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:symbol",
        "//verilog/parser:verilog_token_enum",
    ],
)

cc_test(
    name = "verilog_matchers_test",
    srcs = ["verilog_matchers_test.cc"],
    deps = [
        ":verilog_matchers",
        ":verilog_treebuilder_utils",
        "//common/analysis/matcher",
        "//common/analysis/matcher:core_matchers",
        "//common/analysis/matcher:matcher_builders",
        "//common/analysis/matcher:matcher_test_utils",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "verilog_treebuilder_utils",
    srcs = ["verilog_treebuilder_utils.cc"],
    hdrs = ["verilog_treebuilder_utils.h"],
    deps = [
        ":verilog_nonterminals",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:token_info",
        "//common/util:logging",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "declaration",
    srcs = ["declaration.cc"],
    hdrs = ["declaration.h"],
    deps = [
        ":verilog_nonterminals",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:constants",
        "//common/text:symbol",
        "//common/text:token_info",
        "//common/text:tree_utils",
        "//common/util:container_util",
        "//verilog/parser:verilog_token_enum",
    ],
)

cc_library(
    name = "dimensions",
    srcs = ["dimensions.cc"],
    hdrs = ["dimensions.h"],
    deps = [
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "dimensions_test",
    srcs = ["dimensions_test.cc"],
    deps = [
        ":dimensions",
        ":verilog_matchers",
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/text:tree_utils",
        "//common/util:casts",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "expression",
    srcs = ["expression.cc"],
    hdrs = ["expression.h"],
    deps = [
        ":verilog_nonterminals",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:token_info",
        "//common/text:tree_utils",
        "//common/util:casts",
        "@com_google_absl//absl/strings",
    ],
)

cc_test(
    name = "expression_test",
    srcs = ["expression_test.cc"],
    deps = [
        ":expression",
        ":verilog_nonterminals",
        "//common/text:symbol",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "functions",
    srcs = ["functions.cc"],
    hdrs = ["functions.h"],
    deps = [
        ":verilog_matchers",  # fixdeps: keep
        ":verilog_nonterminals",
        ":verilog_treebuilder_utils",
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "functions_test",
    srcs = ["functions_test.cc"],
    deps = [
        ":functions",
        ":identifier",
        "//common/analysis:syntax_tree_search",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/util:casts",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "//verilog/parser:verilog_token_enum",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "identifier",
    srcs = ["identifier.cc"],
    hdrs = ["identifier.h"],
    deps = [
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
        "//common/util:casts",
        "//common/util:logging",
        "//verilog/parser:verilog_token_enum",
    ],
)

cc_test(
    name = "identifier_test",
    srcs = ["identifier_test.cc"],
    deps = [
        ":identifier",
        "//common/analysis:syntax_tree_search",
        "//common/text:concrete_syntax_leaf",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "module",
    srcs = ["module.cc"],
    hdrs = ["module.h"],
    deps = [
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:token_info",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "module_test",
    srcs = ["module_test.cc"],
    deps = [
        ":module",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/util:casts",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "net",
    srcs = ["net.cc"],
    hdrs = ["net.h"],
    deps = [
        ":identifier",
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "net_test",
    srcs = ["net_test.cc"],
    deps = [
        ":net",
        ":verilog_nonterminals",
        "//common/analysis:syntax_tree_search",
        "//common/text:syntax_tree_context",
        "//common/text:text_structure",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "data",
    srcs = ["data.cc"],
    hdrs = ["data.h"],
    deps = [
        ":identifier",
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "data_test",
    srcs = ["data_test.cc"],
    deps = [
        ":data",
        ":verilog_nonterminals",
        "//common/analysis:syntax_tree_search",
        "//common/text:syntax_tree_context",
        "//common/text:text_structure",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "package",
    srcs = ["package.cc"],
    hdrs = ["package.h"],
    deps = [
        ":verilog_matchers",
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:token_info",
        "//common/text:tree_utils",
        "//common/util:casts",
        "//common/util:status",
    ],
)

cc_test(
    name = "package_test",
    srcs = ["package_test.cc"],
    deps = [
        ":package",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/util:casts",
        "//common/util:logging",
        "//common/util:status",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "parameters",
    srcs = ["parameters.cc"],
    hdrs = ["parameters.h"],
    deps = [
        ":identifier",
        ":verilog_matchers",  # fixdeps: keep
        ":verilog_nonterminals",
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:token_info",
        "//common/text:tree_utils",
        "//common/util:casts",
        "//common/util:logging",
        "//verilog/parser:verilog_token_enum",
        "@com_google_absl//absl/strings",
    ],
)

cc_test(
    name = "parameters_test",
    srcs = ["parameters_test.cc"],
    deps = [
        ":parameters",
        ":verilog_nonterminals",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/util:casts",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "//verilog/parser:verilog_token_enum",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "port",
    srcs = ["port.cc"],
    hdrs = ["port.h"],
    deps = [
        ":identifier",
        ":verilog_matchers",  # fixdeps: keep
        ":verilog_nonterminals",
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
        "//common/util:logging",
    ],
)

cc_test(
    name = "port_test",
    srcs = ["port_test.cc"],
    deps = [
        ":port",
        ":type",
        ":verilog_nonterminals",
        "//common/analysis:syntax_tree_search",
        "//common/text:concrete_syntax_leaf",
        "//common/text:syntax_tree_context",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "seq_block",
    srcs = ["seq_block.cc"],
    hdrs = ["seq_block.h"],
    deps = [
        ":identifier",
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:syntax_tree_context",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "seq_block_test",
    srcs = ["seq_block_test.cc"],
    deps = [
        ":seq_block",
        ":verilog_matchers",
        ":verilog_nonterminals",
        "//common/analysis:syntax_tree_search",
        "//common/text:syntax_tree_context",
        "//common/text:text_structure",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "tasks",
    srcs = ["tasks.cc"],
    hdrs = ["tasks.h"],
    deps = [
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "tasks_test",
    srcs = ["tasks_test.cc"],
    deps = [
        ":identifier",
        ":tasks",
        "//common/analysis:syntax_tree_search",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:text_structure",
        "//common/text:token_info",
        "//common/util:casts",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "//verilog/parser:verilog_token_enum",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "type",
    srcs = ["type.cc"],
    hdrs = ["type.h"],
    deps = [
        ":identifier",
        ":verilog_matchers",  # fixdeps: keep
        "//common/analysis:syntax_tree_search",
        "//common/analysis/matcher",
        "//common/analysis/matcher:matcher_builders",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:tree_utils",
    ],
)

cc_test(
    name = "type_test",
    srcs = ["type_test.cc"],
    deps = [
        ":context_functions",
        ":type",
        "//common/analysis:syntax_tree_search",
        "//common/text:text_structure",
        "//common/util:logging",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "context_functions",
    hdrs = ["context_functions.h"],
    deps = [
        ":verilog_nonterminals",
        "//common/text:syntax_tree_context",
    ],
)

cc_library(
    name = "numbers",
    srcs = ["numbers.cc"],
    hdrs = ["numbers.h"],
    deps = [
        "//common/util:logging",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "verilog_tree_print",
    srcs = ["verilog_tree_print.cc"],
    hdrs = ["verilog_tree_print.h"],
    deps = [
        ":verilog_nonterminals",
        "//common/text:concrete_syntax_leaf",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:token_info",
        "//common/text:tree_utils",
        "//common/util:value_saver",
        "//verilog/parser:verilog_parser",
        "@com_google_absl//absl/strings",
    ],
)

cc_test(
    name = "context_functions_test",
    srcs = ["context_functions_test.cc"],
    deps = [
        ":context_functions",
        ":verilog_nonterminals",
        "//common/text:concrete_syntax_tree",
        "//common/text:symbol",
        "//common/text:syntax_tree_context",
        "//common/text:tree_builder_test_util",
        "//common/util:casts",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "numbers_test",
    srcs = ["numbers_test.cc"],
    deps = [
        ":numbers",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "verilog_tree_print_test",
    srcs = ["verilog_tree_print_test.cc"],
    deps = [
        ":verilog_tree_print",
        "//common/text:symbol",
        "//common/util:status",
        "//verilog/analysis:verilog_analyzer",
        "@com_google_absl//absl/memory",
        "@com_google_googletest//:gtest_main",
    ],
)
