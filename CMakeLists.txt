# Copyright (C) 2019 Intel Corporation. All rights reserved.
# SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception

cmake_minimum_required (VERSION 3.0)

project(xnnpack_wasm)

################  EMCC ################
include(ExternalProject)

ExternalProject_Add(xnnpack
    PREFIX xnnpack
    GIT_REPOSITORY https://github.com/google/XNNPACK.git
    GIT_TAG        master
    GIT_PROGRESS   ON
    SOURCE_DIR     ${CMAKE_CURRENT_SOURCE_DIR}/xnnpack
    UPDATE_COMMAND git checkout .
                   && git reset --hard 4570a7151aa4f3e57eca14a575eeff6bb13e26be
                   && cmake -E copy ${CMAKE_CURRENT_SOURCE_DIR}/xnnpack/google3/third_party/XNNPACK/microkernels.bzl
                      ${CMAKE_CURRENT_SOURCE_DIR}/xnnpack/
                   && git apply ${CMAKE_CURRENT_SOURCE_DIR}/xnnpack.patch
    CONFIGURE_COMMAND ""
    # grep xnnpack_benchmark -A 1 BUILD.bazel \
    #   | grep "name =" \
    #   | awk '{print $3}' \
    #   | sed -e 's/\"//g' -e 's/,//g' -e 's/^/\/\/:/g'
    BUILD_COMMAND  cd ${CMAKE_CURRENT_SOURCE_DIR}/xnnpack
                   && bazel --output_user_root=build-user-output build -c opt --config=wasm
                       //:f32_gemm_bench.js
    INSTALL_COMMAND ${CMAKE_COMMAND} -E copy_directory
                      ${CMAKE_CURRENT_SOURCE_DIR}/xnnpack/bazel-out/wasm-opt/bin/
                      ${CMAKE_BINARY_DIR}/wasm-opt
)
