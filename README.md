# Comparative Study of U-Net, TransUNet, and Diffusion Models for Lung Tumor Segmentation on MSD and LIDC-IDRI

## Overview

This project focuses on lung tumor segmentation from CT images using deep learning models. We compare three architectures:

* U-Net (CNN-based)
* TransUNet (CNN + Transformer hybrid)
* Diffusion-based segmentation model

The goal is to evaluate segmentation performance and analyze generalization across different medical imaging datasets.

---

## Datasets

### 1. Medical Segmentation Decathlon (MSD - Task06 Lung)

* 63 CT volumes with tumor masks
* Converted from 3D volumes to 2D slices (each slice is a single axial CT image)

**Preprocessing:**

* Removed first 30 slices
* Clipped intensities to [-1000, 400]
* Normalized to [0, 1]
* Resized to 256 × 256

**Patient-level split:**

* Train: 50 patients
* Validation: 6 patients
* Test: 7 patients

**Final dataset size:**

* Train: 3551 slices
* Validation: 422 slices
* Test: 524 slices

---

### 2. LIDC-IDRI Dataset

* 875 patients
* 15,548 image-mask pairs
* 2D CT slices (128 × 128)
* Binary masks (tumor vs background)

**Patient-level split:**

* Train: 700 patients
* Validation: 87 patients
* Test: 88 patients

---

## Dataset References

### Medical Segmentation Decathlon (MSD – Task06 Lung)

* Antonelli et al., *The Medical Segmentation Decathlon*, Nature Communications, 2022
* https://www.nature.com/articles/s41467-022-30695-9
* http://medicaldecathlon.com/
* Download: https://drive.google.com/drive/folders/1HqEgzS8BV2c7xYNrZdEAnrHk7osJJ--2

---

### LIDC-IDRI Dataset

* Armato et al., *The Lung Image Database Consortium (LIDC-IDRI)*, Medical Physics, 2011
* https://doi.org/10.1118/1.3528204
* https://www.kaggle.com/datasets/zhangweiled/lidcidri

---

## Models

### U-Net

* Encoder–decoder architecture with skip connections
* Input: 1-channel CT image
* Output: 1-channel tumor mask
* Feature channels: 64 → 128 → 256 → 512

---

### TransUNet

* ResNet34 encoder + Transformer encoder
* Patch size: 1
* Embedding dimension: 256
* Transformer: 4 layers, 4 heads
* Decoder: U-Net-style upsampling with skip connections

---

### Diffusion Model (Proposed)

* Learns to reconstruct tumor masks from noisy inputs
* Uses single-step reconstruction instead of iterative sampling
* Combines noise prediction with segmentation loss

---

## Results

### MSD (Task06 Lung)

| Model     | Dice       | IoU        | Precision | Recall     |
| --------- | ---------- | ---------- | --------- | ---------- |
| U-Net     | 0.7601     | 0.7443     | 0.9758    | 0.7585     |
| TransUNet | 0.7678     | 0.7486     | 0.9202    | 0.7914     |
| Diffusion | **0.8675** | **0.8353** | 0.9007    | **0.9244** |

---

### LIDC-IDRI

| Model     | Dice       | IoU        | Precision | Recall     |
| --------- | ---------- | ---------- | --------- | ---------- |
| U-Net     | 0.6681     | 0.5683     | 0.7290    | 0.7750     |
| TransUNet | 0.6901     | 0.5860     | 0.7263    | 0.8179     |
| Diffusion | **0.7061** | **0.5989** | 0.6470    | **0.9216** |

---

## Repository Structure

```
lung-tumor-segmentation/
├── notebooks/
│   ├── msd.ipynb
│   ├── lidc.ipynb
├── results/
│   ├── msd_preprocessed_slice_167.png
│   ├── lidc_preprocessed_example.png
│   ├── top5_diffusion_predictions_lidc.png
│   ├── top5_diffusion_predictions_with_dice.png
├── README.md
```

---

## Notebooks

* `msd.ipynb`: Training and evaluation on MSD dataset
* `lidc.ipynb`: Training and evaluation on LIDC-IDRI dataset

---

## Key Contributions

* Proposed a diffusion-based segmentation framework using single-step reconstruction
* Compared CNN, Transformer, and diffusion models under the same experimental setting
* Evaluated performance on two medical imaging datasets
* Demonstrated improved Dice score and recall using diffusion-based modeling

---


## How to Run

1. Install dependencies:

```
pip install -r requirements.txt
```

2. Open the notebooks:

* `msd.ipynb`
* `lidc.ipynb`

3. Run all cells sequentially

4. GPU is recommended for faster training


---

## References

1. O. Ronneberger, P. Fischer, and T. Brox,
   *U-Net: Convolutional Networks for Biomedical Image Segmentation*,
   MICCAI, 2015.

2. J. Chen et al.,
   *TransUNet: Transformers Make Strong Encoders for Medical Image Segmentation*,
   arXiv:2102.04306, 2021.

3. J. Ho, A. Jain, and P. Abbeel,
   *Denoising Diffusion Probabilistic Models*,
   NeurIPS, 2020.

4. J. Wu et al.,
   *MedSegDiff: Medical Image Segmentation with Diffusion Probabilistic Model*,
   MIDL, 2024.

5. A. Kazerouni et al.,
   *Diffusion Models in Medical Imaging: A Comprehensive Survey*,
   Medical Image Analysis, 2023.

6. M. Antonelli et al.,
   *The Medical Segmentation Decathlon*,
   Nature Communications, 2022.

7. S. G. Armato III et al.,
   *The Lung Image Database Consortium (LIDC-IDRI)*,
   Medical Physics, 2011.

8. M. Zhang,
   *LIDC-IDRI Dataset (Kaggle Version)*, 2026.
   https://www.kaggle.com/datasets/zhangweiled/lidcidri
