[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)]() [![GraphKAN Site](https://img.shields.io/badge/GraphKAN-Live%20Site-blue?style=for-the-badge)](https://fakeonomics.github.io/graphkan-overview/) [![TernaT](https://img.shields.io/badge/TernaT-Live%20Site-228b22?style=for-the-badge)](https://fakeonomics.github.io/TernaT-overview/)

---

# GraphKAN: Ternary Kolmogorov-Arnold Network

First KAN with **discrete ternary weights** {-1, 0, +1}.  
**1.58 bits/param · 15.4 KB model · 96.15% MNIST · 0 DSP inference**

## Overview

GraphKAN introduces a **universal quantization-aware training pipeline** that converts neural networks to discrete ternary weights {-1, 0, +1} while **improving accuracy** over the float baseline — a regularization-by-quantization effect. The method works on **both KAN and CNN architectures**, achieving:

| Task | Model | Float Accuracy | Ternary Accuracy | Model Size |
|------|-------|:--------------:|:----------------:|:----------:|
| MNIST | GraphKAN 256→100→10 | 94.77% | **96.15%** | **15.4 KB** |
| Fashion-MNIST | CNN conv32→64→FC128→10 | 91.57% | **92.02%** | **102.8 KB** |
| Fashion-MNIST | GraphKAN 256→100→10 | 83.03% | **84.67%** | **15.4 KB** |

## Novel Contributions

1. **First ternary KAN** — All prior KANs use FP32 or 4-bit. Ternary at 1.58 bits/param.
2. **Regularization-by-quantization** — Quantization improves accuracy (94.77% float → 96.15% ternary). The discrete weight space acts as an information bottleneck, forcing the model to learn only robust features.
3. **Universal QAT pipeline** — Same 4-phase pipeline (float → STE → hard clamp → finetune) works on **graph KANs and standard CNNs**. Not architecture-specific.
4. **49% natural sparsity** — Half of ternary weights converge to zero during training, enabling further compression.

## Architecture

- **Graph topology**: Arbitrary directed neuron graph with synchronous update cycles
- **Ternary edges**: Each connection stores a piecewise-linear function with 3 ternary control points {-1, 0, +1}
- **Hardware-efficient**: Weight multiplication is a multiplexer (pass/negate/zero) — zero DSP slices, no FPU required
- **Inference**: 2-3 cycles per weight operation, add/shift only

## Training

4-phase Quantization-Aware Training:

```
Phase 1: Float clamp to range
Phase 2: Gradual ternarization
Phase 3: Hard clamp to {-1, 0, +1}
Phase 4: Finetune scale + bias
```

Total: **15 epochs** on consumer GPU.

## ELM Mode (Zero Hidden Training)

Hidden layer frozen at random ternary weights, output solved via closed-form least squares:

- **99.3%** of full backprop accuracy on Fashion-MNIST (H=500)
- **Zero** hidden layer training required
- 31.6 KB model size

## Hardware Targets

| Target | Memory | Suitability |
|--------|:------:|:-----------:|
| Cortex-M0+ ($0.50 MCU) | 16 KB L1 | ✅ Fits entirely |
| ESP32-S3 | 512 KB SRAM | ✅ + headroom |
| RISC-V GD32V | 32 KB | ✅ |
| Smartwatch DSP | 128 KB | ✅ |

## Status

Proprietary technology — All Rights Reserved.  
Codebase is private. [Full paper](https://fakeonomics.github.io/graphkan-overview/) available.  
For licensing inquiries, contact via GitHub.
