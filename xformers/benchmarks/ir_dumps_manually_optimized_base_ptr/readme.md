Experiment with simplified address computation for K/V tensors in loop.
Add increment to base pointer instead of offset tensor, this reduces amount of computations.

Triton commit: [5df9c723de8c23508773b07fe16dd34e4c444541](https://github.com/ROCm/triton/commit/5df9c723de8c23508773b07fe16dd34e4c444541) (branch [release/internal/3.5.x](https://github.com/ROCm/triton/tree/release/internal/3.5.x))

commands used:

``` bash
# generate IRs in dump directory
TRITON_ALWAYS_COMPILE=1 TRITON_KERNEL_DUMP=1 TRITON_DUMP_DIR=ir_dumps_manually_optimized_base_ptr/ AMDGCN_USE_BUFFER_OPS=1 python3 benchmark_attn_decoding.py

# Run benchmark with modified ir
TRITON_ALWAYS_COMPILE=1 TRITON_KERNEL_OVERRIDE=1 TRITON_OVERRIDE_DIR=ir_dumps_manually_optimized_base_ptr/ AMDGCN_USE_BUFFER_OPS=1 python3 benchmark_attn_decoding.py
```

dump directory contains full set of IRs for modufied variant plus original amdgcn and ttgir dumps.

### Results
. | original | modified
--- | --- | ---
v_add count | 160      | 141
v_mul count | 78       | 74
sgpr count  | 95       | 93
vgpr count  | 338      | 342

Performance did not change
