# PDF Glyph Restoration — Denoising Autoencoder

A minimal deep learning pipeline that restores degraded characters from scanned or poorly printed PDF documents using a convolutional denoising autoencoder.

## Problem

PDF documents produced by low-quality printers or scanners often contain degraded characters — blurred strokes, erosion artifacts, and random noise. This project explores an automated approach to reconstruct clean glyphs from such input.

## Approach

1. **Synthetic dataset generation** — clean glyphs are rendered programmatically from a system font, then degraded via Gaussian blur, morphological erosion, and salt-and-pepper noise to produce (noisy, clean) training pairs.
2. **Denoising autoencoder** — a convolutional encoder-decoder network learns the mapping from degraded to clean glyphs.
3. **Inference** — degraded characters extracted from a real PDF are passed through the trained model to produce restored output.

## Architecture

```
Input (1×64×64)
    → Conv2d(1→32) + ReLU
    → Conv2d(32→64) + ReLU
    → ConvTranspose2d(64→32) + ReLU
    → ConvTranspose2d(32→1) + Sigmoid
Output (1×64×64)
```

## Stack

| Component | Library |
|---|---|
| Glyph rendering | Pillow |
| Degradation pipeline | NumPy, SciPy |
| Model | PyTorch |
| Visualization | Matplotlib |
| Environment | Google Colab |

## Results

The model trained on 26 uppercase Latin characters for 100 epochs achieves MSE loss of ~0.0002 and visually removes noise while preserving glyph shape.

| Input (noisy) | Output (restored) | Target (clean) |
|---|---|---|
| scattered noise + blur | clean glyph | ground truth render |

## Status

Work in progress — shelved for now. Planned next steps:

- extend dataset to Cyrillic alphabet and digits
- add MaxPool2d / Upsample layers for true latent compression
- test on real PDF document crops

## Run in Colab

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPO/blob/main/idk_howtocallit.ipynb)

## License

MIT
