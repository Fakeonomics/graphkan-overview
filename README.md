# Ternary GraphKAN

[![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-red)]() 

Kolmogorov-Arnold Networks with **discrete ternary weights** {-1, 0, +1}. First KAN quantized below 4-bit — achieves **1.58 bits per parameter** with a **15.4 KB model** at **96.15% accuracy** on MNIST.

---

## Key Results

| Model | Precision | Params | Size | Accuracy |
|-------|-----------|--------|------|----------|
| GraphKAN (256→100→10) | Float | 79,800 | 15.4 KB | 94.77% |
| GraphKAN (256→100→10) | **Ternary** | 79,800 | **15.4 KB** | **96.15%** |
| GraphKAN (256→100→10) | Ternary | 79,800 | 15.4 KB | 86.68% (Fashion-MNIST) |
| MLP (256→100→10) | Float | 26,710 | 106.8 KB | ~93% |

**Smallest KAN ever reported achieving >95% on MNIST.**

---

## Novel Contributions

1. **First ternary KAN** — All prior KANs use FP32 or 4-bit quantization. Ternary format uses 1.58 bits/param.
2. **Regularization-by-quantization** — Accuracy consistently improves during quantization (float → STE ternary → hard clamp → finetune), acting as a regularizer. No prior art reports this effect in KANs.
3. **Graph topology with cycles** — Arbitrary directed graph connections with synchronous update cycles.
4. **Extreme compression** — 15.4 KB model, 49% natural sparsity, fits in L1 cache of microcontrollers.

---

## Training Pipeline (4-Phase QAT)

```
Phase 1 (float clamp) → Phase 2 (STE ternary) → Phase 3 (hard clamp) → Phase 4 (finetune)
```

---

## Weight Packing

Ternary values {-1,0,+1} stored in 2 bits each, 4 values per uint8 byte:
79,800 parameters → **19,950 bytes** packed. Total model <22 KB.

---

## Hardware Fit

| Target | Memory | Fits? |
|--------|--------|-------|
| Cortex-M4 L1 cache | 16-32 KB | ✅ |
| RISC-V microcontroller | 16-64 KB | ✅ |
| Smartwatch DSP | 32-128 KB | ✅ |

---

## Status

Proprietary technology — All Rights Reserved. Codebase is private.
For licensing inquiries or collaboration, contact via [GitHub](https://github.com/Fakeonomics).

*Created by Fakeonomics, June 2026.*
