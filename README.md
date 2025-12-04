# Cytoplasmic Strings Analysis in Human Embryo Time-Lapse Videos using Deep Learning Framework

This repository contains the official implementation for the paper:

> **Cytoplasmic Strings Analysis in Human Embryo Time-Lapse Videos using Deep Learning Framework**  

The project provides the first computational framework for **automatic analysis of cytoplasmic strings (CS)** in human IVF embryo time-lapse (TLI) videos, including:

- A **human-in-the-loop annotation pipeline** to build the first CS dataset  
- A **two-stage deep learning framework** (classification → detection)  
- A new loss function **NUCE (Novel Uncertainty-aware Contractive Embedding)**  
- A **state-of-the-art RF-DETR–based detector** for extremely thin, low-contrast CS structures

![CS_Detection](/images/1.png)
![CS_Detection](/images/2.png)
---

## 🔍 Problem & Motivation

- Infertility is a major global health issue; IVF success rates remain **~30–50%**.  
- Time-lapse imaging (TLI) enables continuous, non-invasive monitoring of embryo development, but most automated systems still rely mainly on **conventional morphokinetics**.  
- **Cytoplasmic Strings (CS)** are thin filamentous structures connecting the inner cell mass (ICM) and trophectoderm (TE) in expanded blastocysts.  
  - Presence and activity of CS have been associated with:
    - Faster blastocyst formation  
    - Higher blastocyst grades  
    - Improved viability and live-birth potential  

Today, CS assessment is done **manually** by embryologists from TLI videos – this is subjective, slow, and very hard due to:

- Very **sparse** CS-positive frames  
- **Thin, low-contrast**, and highly variable CS morphology  
- Transient appearance across focal planes  
- Strong **class imbalance** (CS− ≫ CS+)

This repository tackles those issues directly.

---

## 📊 Dataset & Annotation Pipeline

We build the first expert-validated CS dataset from human embryo TLI videos using a **human-in-the-loop annotation pipeline**:

1. **Initial manual annotation**  
   - Expert embryologists manually annotate a small subset of blastocyst-stage frames with visible CS.
   - This forms a **biologically verified support set**.

2. **Model-assisted auto-annotation**  
   - A preliminary detector (RF-DETR) is trained on this support set.  
   - It predicts candidate CS regions (bounding boxes / probability maps) on unseen TLI frames.

3. **Expert verification and refinement**  
   - Embryologists review each candidate region: accept / refine / reject.  
   - Additional CS instances are added where the model missed them.  
   - Each frame is verified by **at least two experts** to reduce inter-observer variability.

4. **Iterative refinement**  
   - Verified labels are merged back into the training set; the detector is retrained.  
   - The cycle (predict → verify → retrain) is repeated until convergence.

**Final dataset summary (as described in the paper):**

- **90** developmental sequences  
- **13,568** frames total  
- Only **271 frames** contain visible CS (extreme class imbalance)  
- For each frame:
  - Binary **CS presence label** (CS+ / CS−)  
  - For CS+ frames: **bounding boxes** for CS regions  

> To the best of our knowledge, this is the **first dedicated dataset for cytoplasmic string detection and localization** in human IVF embryos.

---

## 🧠 Method Overview

The overall framework is **two-stage** (see Fig. 2 in the paper):

1. **Stage 1 – CS Presence Classification (Frame-level)**  
   - Input: single blastocyst-stage frame  
   - Output: probability that CS are present in the frame  
   - Backbones evaluated:
     - ViT-B  
     - Swin-B  
     - DeiT-B  
     - DINOv2-B  
     - CLIP-B/32  
   - Optimized with the proposed **NUCE loss**, which:
     - Reweights **uncertain samples** (uncertainty-aware risk term)  
     - Enforces **compact, well-separated feature clusters** via contractive embedding toward class anchors  

   Frames with CS presence probability ≥ τ are passed to stage 2.

2. **Stage 2 – CS Localization (Detection)**  
   - Input: CS-positive frames (from stage 1)  
   - Output: bounding boxes localizing CS regions  
   - Detectors evaluated:
     - YOLOv8 (n/s/m/l/x)  
     - YOLOv11 (n/s/m/l/x)  
     - YOLOv12 (n/s/m/l/x)  
     - RT-DETR (l/x)  
     - RT-DETRv2 (s/m/l)  
     - **RF-DETR (n/s/m/b/l)**  

   **RF-DETR-m** achieves the best performance, with strong mAP@0.25 / mAP@0.50 / mAP@0.75, and is used as the main detector in the framework.

---

## 🧪 Key Technical Contributions

1. **First computational framework for CS analysis in IVF embryos**  
   - Human-in-the-loop pipeline + curated CS dataset  
   - Two-stage architecture that separates **presence classification** and **fine-grained localization**

2. **NUCE Loss – Novel Uncertainty-aware Contractive Embedding**  
   - Classification loss designed for severe imbalance and subtle features  
   - Two components:
     - **Uncertainty-aware weighting**:  
       \( \omega_i = (1 - \max_k p_{i,k})^\gamma \) up-weights ambiguous samples.  
     - **Contractive embedding term**:  
       pulls features toward class-specific anchors to form compact, discriminative clusters.  
   - Implemented both in **sample-wise** form and a **matrix form** for efficient training.  
   - Consistently improves F1 across all tested transformers (ViT-B, Swin-B, DeiT-B, DINOv2-B, CLIP-B/32).

3. **State-of-the-art detection of extremely thin CS structures**  
   - RF-DETR significantly outperforms YOLO variants and vanilla RT-DETR / RT-DETRv2, especially at high IoU thresholds, which are crucial for precise localization of filamentous structures.

---

## 📁 Repository Structure

The repository is organized to mirror the two-stage framework:

```text
CS_Detection/
├── classification/     # Notebooks / code for CS presence classification (Stage 1)
├── detection/          # Notebooks / code for CS localization (Stage 2)
├── README.md           # This file
└── (data not included) # See paper for dataset description
