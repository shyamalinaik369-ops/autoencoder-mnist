# autoencoder-mnist
Autoencoders on MNIST | Neural Networks &amp; Latent Space Analysis
# Analysing Latent Representations Using Autoencoders on MNIST

> Unsupervised Deep Learning · TensorFlow · Keras · Latent Space Analysis

---

## The Idea

Training an autoencoder is straightforward. Understanding *what it actually learned* is the hard part.

This project goes far beyond standard reconstruction tutorials. After training a dense autoencoder on 70,000 MNIST handwritten digits, the focus shifts entirely to **interrogating the latent space** — probing what 32 numbers know about a 784-pixel image.

---

## Key Results

| Metric | Value |
|---|---|
| Input dimensions | 784 (28×28 pixels) |
| Latent dimensions | 32 |
| Compression ratio | **24.5×** |
| Training loss (epoch 1 → 20) | 0.3324 → **0.0909** |
| Validation loss (epoch 1 → 20) | 0.1596 → **0.0897** |
| Overfitting | None — val loss tracks train loss throughout |

---

## Architecture

```
Input (784) → Dense 128 (ReLU) → Dense 64 (ReLU) → Latent (32)
                                                         ↓
Output (784) ← Dense 128 (ReLU) ← Dense 64 (ReLU) ← Latent (32)

Loss: Binary Cross-Entropy
Optimiser: Adam
Epochs: 20 · Batch size: 256
```

The bottleneck layer (32 neurons) is the constraint that gives the system meaning. If the decoder can reconstruct a digit from only 32 numbers, those 32 numbers must encode something real.

---

## Phase 2: Latent Space Analysis (The Interesting Part)

### t-SNE Projection
Projecting the 10,000 test images' latent vectors into 2D reveals that the autoencoder — **trained entirely without labels** — arranged the 10 digit classes into separable clusters. Digits 0, 1, and 6 form tight, distinct clusters. Digits 3, 5, and 8 overlap — because they share visual sub-features (curves, loops, rounded strokes).

### Neuron Activation Analysis
Examining which neurons activate for which digits reveals the central insight: **neurons don't encode digits — they encode features of digits**. Neuron 8 responds most strongly to images of digit 8 — but also to some digit 9 images, because both share circular loop structures. The latent space encodes *reusable visual components*, not class-specific templates.

### Single Neuron Manipulation
Taking a digit 7's latent vector and varying Neuron 5 from −3.0 to +3.0: the top stroke of the 7 gradually curves, then straightens. The digit never stops being a 7. **This neuron controls stroke style, not digit identity.**

### Digit Interpolation
Linearly interpolating between the latent vectors of digit 2 and digit 9 produces a smooth morphing sequence — the upper curve of the 2 elongates, the circular base of the 9 emerges. **The latent space is continuous and semantically organised.**

### Perturbation Sensitivity
Perturbing each of the 32 neurons by +1.0 independently reveals uneven impact: neurons N8, N15, N24 cause noticeable shape changes; N1, N9, N12 have near-zero effect. **Encoded information is not distributed equally.**

---

## The Central Insight

> *"The autoencoder learns the DNA of digits — not the digits themselves. Individual neurons encode reusable visual components: loops, curves, stroke orientations. A digit is assembled from these components, much like a letter is assembled from strokes."*

---

## An Honest Note on Stochasticity

When the code was re-run with identical architecture and data, the specific neuron indices that were most active changed completely. This was noticed, understood, and documented — not hidden. The cause: random weight initialisation + Adam's stochastic gradient estimates + non-convex loss landscape = different local minima, same reconstruction quality. The overall encoding capability remains consistent across runs; only the specific neuron assignments change.

---

## Tech Stack

| Tool | Usage |
|---|---|
| Python | Core implementation |
| TensorFlow / Keras | Autoencoder architecture and training |
| NumPy | Latent vector manipulation |
| Matplotlib | All visualisations |
| Scikit-Learn | t-SNE projection |

---

## Repository Structure

```
autoencoder-mnist/
├── autoencoder_mnist.ipynb    # Main analysis notebook
├── README.md
└── LICENSE
```

*MNIST data is loaded automatically via `tensorflow.keras.datasets.mnist` — no manual download needed.*

---

## How to Run

```bash
pip install tensorflow numpy matplotlib scikit-learn

jupyter notebook autoencoder_mnist.ipynb
```

---

## Author

**Shyamali Naik** · MSc Data Analytics · Queen Mary University of London · 2025

[LinkedIn](https://www.linkedin.com/in/shyamali-naik-91n78) · [Portfolio](https://shyamalinaik369-ops.github.io)

---

*© 2025 Shyamali Naik. Licensed under MIT.*
