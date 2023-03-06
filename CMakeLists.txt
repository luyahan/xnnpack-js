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
                      //:qs8_dwconv_bench.js
                       //:qs8_f32_vcvt_bench.js
                       //:qs8_gemm_bench.js
                       //:qs8_requantization_bench.js
                       //:qs8_vadd_bench.js
                       //:qs8_vaddc_bench.js
                       //:qs8_vcvt_bench.js
                       //:qs8_vlrelu_bench.js
                       //:qs8_vmul_bench.js
                       //:qs8_vmulc_bench.js
                       //:qu8_f32_vcvt_bench.js
                       //:qu8_gemm_bench.js
                       //:qu8_requantization_bench.js
                       //:qu8_vadd_bench.js
                       //:qu8_vaddc_bench.js
                       //:qu8_vcvt_bench.js
                       //:qu8_vlrelu_bench.js
                       //:qu8_vmul_bench.js
                       //:qu8_vmulc_bench.js
                       //:bf16_gemm_bench.js
                       //:f16_igemm_bench.js
                       //:f16_gemm_bench.js
                       //:f16_raddstoreexpminusmax_bench.js
                       //:f16_spmm_bench.js
                       //:f16_vsigmoid_bench.js
                       //:f16_f32_vcvt_bench.js
                       //:f32_igemm_bench.js
                       //:f32_conv_hwc_bench.js
                       //:f16_conv_hwc2chw_bench.js
                       //:f16_gavgpool_cw_bench.js
                       //:f32_gavgpool_cw_bench.js
                       //:f32_conv_hwc2chw_bench.js
                       //:f16_dwconv_bench.js
                       //:f32_dwconv_bench.js
                       //:f32_dwconv2d_chw_bench.js
                       //:f16_dwconv2d_chw_bench.js
                       //:f32_f16_vcvt_bench.js
                       //:xx_transpose_bench.js
                       //:x8_transpose_bench.js
                       //:x16_transpose_bench.js
                       //:x24_transpose_bench.js
                       //:x32_transpose_bench.js
                       //:x64_transpose_bench.js
                       //:f32_gemm_bench.js
                       //:f32_qs8_vcvt_bench.js
                       //:f32_qu8_vcvt_bench.js
                       //:f32_raddexpminusmax_bench.js
                       //:f32_raddextexp_bench.js
                       //:f32_raddstoreexpminusmax_bench.js
                       //:f32_rmax_bench.js
                       //:f32_spmm_bench.js
                       //:f32_softmax_bench.js
                       //:f16_velu_bench.js
                       //:f32_velu_bench.js
                       //:f32_vhswish_bench.js
                       //:f32_vlrelu_bench.js
                       //:f32_vrelu_bench.js
                       //:f32_vscaleexpminusmax_bench.js
                       //:f32_vscaleextexp_bench.js
                       //:f32_vsigmoid_bench.js
                       //:f16_vsqrt_bench.js
                       //:f32_vsqrt_bench.js
                       //:f32_im2col_gemm_bench.js
                       //:rounding_bench.js
                       //:s16_rmaxabs_bench.js
                       //:s16_window_bench.js
                       //:u32_filterbank_accumulate_bench.js
                       //:u32_filterbank_subtract_bench.js
                       //:u32_vlog_bench.js
                       //:u64_u32_vsqrtshift_bench.js
                       //:i16_vlshift_bench.js
                       //:cs16_vsquareabs_bench.js
                       //:cs16_bfly4_bench.js
                       //:cs16_fftr_bench.js
                       //:x8_lut_bench.js
                       //:abs_bench.js
                       //:average_pooling_bench.js
                       //:bankers_rounding_bench.js
                       //:ceiling_bench.js
                       //:channel_shuffle_bench.js
                       //:convert_bench.js
                       //:convolution_bench.js
                       //:deconvolution_bench.js
                       //:elu_bench.js
                       //:floor_bench.js
                       //:global_average_pooling_bench.js
                       //:hardswish_bench.js
                       //:leaky_relu_bench.js
                       //:max_pooling_bench.js
                       //:negate_bench.js
                       //:sigmoid_bench.js
                       //:prelu_bench.js
                       //:softmax_bench.js
                       //:square_bench.js
                       //:square_root_bench.js
                       //:truncation_bench.js
                       //:f16_gemm_e2e_bench.js
                       //:f32_dwconv_e2e_bench.js
                       //:f32_gemm_e2e_bench.js
                       //:qs8_dwconv_e2e_bench.js
                       //:qs8_gemm_e2e_bench.js
                       //:qu8_gemm_e2e_bench.js
                       //:qu8_dwconv_e2e_bench.js
                       //:end2end_bench.js
                       //:f16_exp_ulp_eval.js
                       //:f16_expminus_ulp_eval.js
                       //:f16_expm1minus_ulp_eval.js
                       //:f16_sigmoid_ulp_eval.js
                       //:f16_sqrt_ulp_eval.js
                       //:f32_exp_ulp_eval.js
                       //:f32_expminus_ulp_eval.js
                       //:f32_expm1minus_ulp_eval.js
                       //:f32_extexp_ulp_eval.js
                       //:f32_sigmoid_ulp_eval.js
                       //:f32_sqrt_ulp_eval.js
                       //:f32_tanh_ulp_eval.js
    INSTALL_COMMAND ${CMAKE_COMMAND} -E copy_directory
                      ${CMAKE_CURRENT_SOURCE_DIR}/xnnpack/bazel-out/wasm-opt/bin/
                      ${CMAKE_BINARY_DIR}/wasm-opt
)
