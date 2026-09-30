# 2D DCT-II Block-Based Image Compression

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg?logo=python&logoColor=white)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.24%2B-013243.svg?logo=numpy&logoColor=white)](https://numpy.org/)
[![SciPy](https://img.shields.io/badge/SciPy-1.10%2B-8CAAE6.svg?logo=scipy&logoColor=white)](https://scipy.org/)
[![Pillow](https://img.shields.io/badge/Pillow-10.x-green.svg)](https://python-pillow.org/)
[![CustomTkinter](https://img.shields.io/badge/GUI-CustomTkinter-blueviolet.svg)](https://customtkinter.tomschimansky.com/)

> Mathematical implementation, algorithmic benchmarking, and interactive GUI application for block-based image compression using the **2D Discrete Cosine Transform (DCT-II)**. Developed for the *Scientific Computing Methods* (*Metodi del Calcolo Scientifico*) Master's course at Università degli Studi di Milano - Bicocca.

---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Mathematical Foundation](#-mathematical-foundation)
  - [1D Discrete Cosine Transform (DCT-II)](#1-1d-discrete-cosine-transform-dct-ii)
  - [2D Separable Transform](#2-2d-separable-transform)
  - [Block-Based Frequency Masking](#3-block-based-frequency-masking)
- [Software Components](#-software-components)
  - [Interactive GUI Application (`app.py`)](#1-interactive-gui-application-apppy)
  - [Compression Engine (`compressor.py`, `dct.py`)](#2-compression-engine)
  - [Performance Benchmarking (`benchmark.py`)](#3-performance-benchmarking-benchmarkpy)
- [Quick Start](#-quick-start)
- [Repository Structure](#-repository-structure)
- [Course Report & Authors](#-course-report--authors)

---

## 🔬 Project Overview

The Discrete Cosine Transform (DCT-II) is the fundamental mathematical transform powering modern lossy image and video compression standards (including JPEG and MPEG). Because natural images exhibit strong spatial correlation among neighboring pixels, transforming pixel blocks into the spatial frequency domain concentrates energy into a small number of low-frequency coefficients. High-frequency spatial variations (to which human vision is less sensitive) can then be discarded, achieving substantial data reduction with minimal perceptible degradation.

This repository features:
1. **Mathematical implementation from scratch**: Constructing orthonormal DCT matrices and evaluating tensor contractions ($C = D f D^T$).
2. **Fast transform optimization**: Leveraging $O(N \log N)$ FFT-based algorithms via `scipy.fftpack`.
3. **Interactive Desktop GUI**: A `CustomTkinter` desktop application enabling real-time experimentation with block size ($F$) and frequency cutoff threshold ($d$).
4. **Computational benchmarks**: Measuring runtime scaling across block dimensions.

---

## 📐 Mathematical Foundation

### 1. 1D Discrete Cosine Transform (DCT-II)
The orthonormal DCT-II transformation matrix $D \in \mathbb{R}^{N \times N}$ is defined by:
$$D_{k, i} = \alpha_k \cos\left(\frac{k \pi (2i + 1)}{2N}\right), \quad i, k \in \{0, \dots, N-1\}$$

where the normalization factor $\alpha_k$ ensures orthonormality ($D D^T = I$):
$$\alpha_0 = \sqrt{\frac{1}{N}}, \quad \alpha_k = \sqrt{\frac{2}{N}} \quad \text{for } k > 0$$

Given a 1D vector $f \in \mathbb{R}^N$, the transformed coefficients $c$ and reconstructed vector $\hat{f}$ are computed via matrix multiplication:
$$c = D f, \qquad \hat{f} = D^T c$$

### 2. 2D Separable Transform
Because the 2D DCT-II kernel is separable, transforming an $N \times N$ matrix $f$ is equivalent to applying 1D transforms along columns, then along rows:
$$C = D f D^T, \qquad \hat{f} = D^T C D$$

### 3. Block-Based Frequency Masking
1. The input image is divided into disjoint $F \times F$ pixel blocks (typically $F \in \{8, 16, 32, 64\}$).
2. For each block $f$, the 2D frequency matrix $C = \text{DCT2}(f)$ is computed.
3. Frequency cutoff criterion: Coefficients $C_{k, l}$ satisfying $(k + l) \ge d$ are zeroed out (where $d \in [0, 2F - 2]$):
   $$C_{k, l} \leftarrow 0 \quad \text{if } k + l \ge d$$
4. Reconstruct each block via inverse transform $\hat{f} = \text{IDCT2}(C)$ and assemble the compressed image.

---

## 💻 Software Components

### 1. Interactive GUI Application (`app.py`)
Built using modern `CustomTkinter` widgets:
- Load any standard `.bmp` image from disk.
- Interactively adjust block size $F$ and frequency cutoff threshold $d$ using dynamic sliders.
- Instantaneous side-by-side visual comparison between original and reconstructed images.
- Real-time calculation of compression statistics (discarded coefficient percentage).

```bash
python3 app.py
```

### 2. Compression Engine
- `dct.py`: Contains both naive $O(N^3)$ matrix-multiplication DCT (`custom_dct2`) and fast $O(N^2 \log N)$ FFT-based implementations (`fast_dct2`, `fast_idct2`).
- `compressor.py`: Modular block partitioner, vectorizer, and frequency thresholding pipeline.

### 3. Performance Benchmarking (`benchmark.py`)
Compares wall-clock execution times of custom matrix multiplication against optimized `scipy.fftpack` across variable matrix sizes ($N = 8, 16, 32, 64, \dots$).

```bash
python3 benchmark.py
```

---

## 🚀 Quick Start

### 1. Prerequisites
- Python 3.10 or higher

### 2. Installation
```bash
git clone https://github.com/mbroglio/ImageCompression.git
cd ImageCompression

# Create virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Launch the Application
```bash
python3 app.py
```

---

## 📁 Repository Structure

```text
├── Assignment2_Report.pdf        # Full academic report (PDF)
├── README.md                     # Project documentation (English)
├── requirements.txt              # Python dependencies
├── app.py                        # CustomTkinter graphical user interface
├── compressor.py                 # Block-based image compression core
├── dct.py                        # 1D/2D DCT-II and IDCT-II algorithms
├── benchmark.py                  # Runtime benchmark comparing naive vs. fast DCT
├── test_matrices.py              # Unit tests for DCT matrix orthonormality
├── validate_compression.py       # Validation and numerical verification script
└── images/                       # Sample test benchmark images (.bmp)
    ├── cathedral.bmp
    ├── deer.bmp
    ├── bridge.bmp
    ├── shoe.bmp
    └── ...
```

---

## 🎓 Course Report & Authors

Project developed for the **Scientific Computing Methods** (*Metodi del Calcolo Scientifico*) course (Academic Year 2025/2026), Master's Degree in Computer Science, **Università degli Studi di Milano - Bicocca**:

- **Matteo Broglio** - Matricola `899562`
- **Lorenzo Caputo** - Matricola `894528`
- **Daniel Giuggioli** - Matricola `894415`

Full report available in [Assignment2_Report.pdf](Assignment2_Report.pdf).
