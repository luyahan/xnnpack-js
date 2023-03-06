

# This repo fork from [wasm-micro-runtime](https://github.com/bytecodealliance/wasm-micro-runtime/blob/27e7e160af54fe8a74620c4fbfacad480b5cb00c/samples/workload/XNNPACK)

"XNNPACK" sample introduction
==============

This sample demonstrates how to build [XNNPACK](https://github.com/google/XNNPACK) benchmarks into WebAssembly with emsdk toolchain and run them with iwasm.

## Installation toolchains

please refer to [installation instructions](../README.md).

## Build XNNPACK

```bash
cd <wamr-dir>/samples/workload/XNNPACK
mkdir build
cd build
cmake ..
make
```
The wasm files are generated under folder samples/workload/XNNPACK/xnnpack/bazel-bin.

