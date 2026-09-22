# DATA 266 HW2.5 Metrics

## GPU Provenance

- GPU: NVIDIA GeForce RTX 4090
- GPU UUID: `GPU-3b8e816c-80f8-6ffd-b5ba-72725de33e1f`
- Architecture: Ada Lovelace
- VRAM: 24,564 MiB
- Driver: 610.60
- CUDA reported by NVIDIA-SMI: 13.3
- PyTorch: 2.11.0+cu128
- PyTorch CUDA: 12.8
- Compute capability: 8.9
- Attention configuration: batch size 1, one head, head dimension 64, FP16

## Table HW2.5.1 — Summary

| Measurement | RTX 4090 | Notes |
|---|---:|---|
| Peak achieved TFLOPS (BF16) | 161.320 TFLOPS | N = 16,384 |
| Percentage of theoretical peak (BF16) | 97.65% | Dense BF16 peak with FP32 accumulation: 165.2 TFLOPS |
| Effective bandwidth | 908.65 GB/s | FP32 elementwise addition |
| Bandwidth as percentage of specification | 90.14% | NVIDIA specification: 1008 GB/s |
| Naive attention OOM length | 49,152 succeeded; 65,536 failed | Tested boundary interval |
| Fused attention OOM length | No OOM through 1,048,576 | Exact failure boundary not reached |
| Steady-state / peak throughput | 97.9% | 150.636 / 153.867 TFLOPS |
| Throttle onset | None detected | Maximum temperature: 76°C |

## Important Notes

The naive attention implementation materialized the full sequence-by-sequence attention matrix. Its memory usage therefore grew approximately quadratically with sequence length.

The fused implementation avoided explicitly storing the complete attention matrix. This substantially reduced peak memory usage and allowed the tested sequence length to reach 1,048,576 without an out-of-memory failure.

The fused result is reported as a lower bound because the exact failure boundary was not reached.

## Source

The RTX 4090 specifications and dense theoretical peak rates were taken from NVIDIA's Ada GPU Architecture Whitepaper, Appendix A, Table 2:

https://images.nvidia.com/aem-dam/Solutions/Data-Center/l4/nvidia-ada-gpu-architecture-whitepaper-V2.02.pdf