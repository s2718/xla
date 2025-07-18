load(
    "//xla/tsl:tsl.bzl",
    "if_google",
)
load("//xla/tsl/platform:rules_cc.bzl", "cc_library")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = ["//visibility:public"],
    licenses = ["notice"],
)

cc_library(
    name = "interpreter_transfer_manager",
    srcs = ["interpreter_transfer_manager.cc"],
    hdrs = ["interpreter_transfer_manager.h"],
    deps = [
        ":platform_id",
        "//xla/service:generic_transfer_manager",
        "//xla/service:shaped_buffer",
        "//xla/service:transfer_manager",
        "//xla/stream_executor:device_memory",
        "//xla/stream_executor:stream_executor_h",
    ],
    alwayslink = True,  # Contains per-platform transfer manager registration
)

cc_library(
    name = "compiler",
    srcs = ["compiler.cc"],
    hdrs = ["compiler.h"],
    deps = [
        ":executable",
        ":platform_id",
        "//xla:literal",
        "//xla:status_macros",
        "//xla:util",
        "//xla/hlo/evaluator:hlo_evaluator",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/ir:hlo_module_group",
        "//xla/hlo/pass:hlo_pass_pipeline",
        "//xla/hlo/transforms/expanders:cholesky_expander",
        "//xla/hlo/transforms/expanders:dynamic_index_splitter",
        "//xla/hlo/transforms/expanders:eigh_expander",
        "//xla/hlo/transforms/expanders:qr_expander",
        "//xla/service:batchnorm_expander",
        "//xla/service:compiler",
        "//xla/service:computation_placer",
        "//xla/service:custom_call_target_registry",
        "//xla/service:dynamic_dimension_inference",
        "//xla/service:executable",
        "//xla/service:hlo_cost_analysis",
        "//xla/service:hlo_module_config",
        "//xla/service:layout_assignment",
        "//xla/service:topk_rewriter",
        "//xla/service:triangular_solve_expander",
        "//xla/stream_executor:platform",
        "//xla/stream_executor:stream_executor_h",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/types:span",
        "@tsl//tsl/platform:errors",
        "@tsl//tsl/platform:status",
        "@tsl//tsl/platform:statusor",
    ],
    alwayslink = True,  # Contains compiler registration
)

cc_library(
    name = "platform_id",
    srcs = ["platform_id.cc"],
    hdrs = ["platform_id.h"],
    deps = ["//xla/stream_executor:platform"] + if_google(
        ["@com_google_protobuf//:any_cc_proto"],
        ["@com_google_protobuf//:protobuf_headers"],
    ),
)

cc_library(
    name = "executable_base",
    srcs = ["executable_base.cc"],
    hdrs = ["executable_base.h"],
    deps = [
        "//xla:literal",
        "//xla:shape_tree",
        "//xla:shape_util",
        "//xla:status_macros",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla:xla_proto_cc",
        "//xla/hlo/ir:hlo",
        "//xla/service:dynamic_dimension_inference",
        "//xla/service:executable",
        "//xla/service:maybe_owning_device_memory",
        "//xla/service:shaped_buffer",
        "//xla/service:transfer_manager",
        "//xla/stream_executor:device_memory",
        "//xla/stream_executor:device_memory_allocator",
        "//xla/stream_executor:platform",
        "//xla/stream_executor:stream",
        "//xla/stream_executor:stream_executor_h",
        "//xla/tsl/platform:env",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "executable",
    srcs = ["executable.cc"],
    hdrs = ["executable.h"],
    deps = [
        ":executable_base",
        "//xla:literal",
        "//xla:shape_util",
        "//xla:types",
        "//xla:xla_data_proto_cc",
        "//xla/hlo/evaluator:hlo_evaluator",
        "//xla/hlo/ir:hlo",
        "//xla/service:dynamic_dimension_inference",
        "//xla/service:executable",
        "//xla/service:hlo_cost_analysis",
        "//xla/service:hlo_execution_profile",
        "//xla/service:hlo_module_config",
        "//xla/service:shaped_buffer",
        "//xla/stream_executor:stream_executor_h",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:span",
    ],
)

cc_library(
    name = "platform",
    srcs = ["platform.cc"],
    hdrs = ["platform.h"],
    deps = [
        ":executor",
        ":platform_id",
        "//xla/stream_executor:device_description",
        "//xla/stream_executor:executor_cache",
        "//xla/stream_executor:platform",
        "//xla/stream_executor:platform_manager",
        "//xla/stream_executor:stream_executor_h",
        "//xla/stream_executor/platform:initialize",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:str_format",
        "@tsl//tsl/platform:status",
        "@tsl//tsl/platform:statusor",
    ],
    alwayslink = True,  # Registers itself with the PlatformManager.
)

cc_library(
    name = "executor",
    srcs = ["executor.cc"],
    hdrs = ["executor.h"],
    deps = [
        "//xla:xla_data_proto_cc",
        "//xla/stream_executor:device_description",
        "//xla/stream_executor:device_memory",
        "//xla/stream_executor:event",
        "//xla/stream_executor:generic_memory_allocation",
        "//xla/stream_executor:generic_memory_allocator",
        "//xla/stream_executor:memory_allocation",
        "//xla/stream_executor:memory_allocator",
        "//xla/stream_executor:platform",
        "//xla/stream_executor:stream",
        "//xla/stream_executor:stream_executor_common",
        "//xla/stream_executor:stream_executor_h",
        "//xla/stream_executor/host:host_stream",
        "@com_google_absl//absl/functional:any_invocable",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/log:check",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings:str_format",
    ],
)
