# HTR-MB: Combining Mamba and BiLSTM for Handwritten Text Recognition

**Official PyTorch implementation of _HTR-MB: Combining Mamba and BiLSTM for Handwritten Text Recognition_, accepted in Pattern Recognition.**

**Filippo Botti, Vittorio Bernuzzi, Tomaso Fontanini, Massimo Bertozzi, Andrea Prati**  
University of Parma, Italy

---

## Overview

HTR-MB is a line-level Handwritten Text Recognition architecture that combines **Mamba** and **Bidirectional LSTMs** under a CTC-based recognition framework.

The key idea is to separate sequence modeling into two complementary stages:

- **Bidirectional Mamba (BDM)** efficiently models the ordered visual sequence and long-range dependencies.
- **BiLSTM decoding head** refines the resulting features by strengthening local and short-range temporal dependencies before CTC prediction.

This combination provides a sequence inductive bias well suited to handwritten text and achieves state-of-the-art recognition accuracy on **IAM**, **READ2016**, and **LAM**, without requiring large-scale synthetic pretraining or Transformer-specific regularization.

<p align="center">
  <img src="img/arch.png" width="900">
</p>

---

## Visual Results
<p align="center">
  <img src="img/results.png" width="900">
</p>

---

## Results

HTR-MB achieves state-of-the-art CER and WER on all three evaluated benchmarks.

| Dataset | CER ↓ | WER ↓ |
|:---|---:|---:|
| **IAM** | **4.42** | **14.01** |
| **READ2016** | **3.49** | **15.07** |
| **LAM** | **2.69** | **7.15** |

The final architecture consists of a **ResNet-18 feature extractor**, **4 Bidirectional Mamba blocks**, a **2-layer BiLSTM decoding head**, and **CTC loss**.

---

## Installation

Clone the repository and create the environment:

```bash
git clone https://github.com/FilippoBotti/HTR-MB.git
cd HTR-MB

conda create -n htr python=3.11
conda activate htr

pip install -r requirements.txt
```

---

## Pretrained Models

Pretrained checkpoints for the evaluated datasets are available here:

**[Download checkpoints](https://univpr-my.sharepoint.com/:f:/g/personal/filippo_botti_unipr_it/IgAhBzs3RtayTIDAMb9BgsZdAcW-UHsTsbgmDFgSUdPVIoo?e=9siZr6)**

---

## Datasets

We evaluate HTR-MB on:

- **IAM** — modern English handwriting
- **READ2016** — historical German manuscripts
- **LAM** — historical Italian manuscripts

Datasets should be placed under `./data/`.

For example:

```text
data/
└── iam/
    ├── train.ln
    ├── val.ln
    ├── test.ln
    └── lines/
        ├── a01-000u-00.png
        ├── a01-000u-00.txt
        ├── a01-000u-01.png
        ├── a01-000u-01.txt
        └── ...
```

</summary>
  <details>
   <summary>
   IAM
   </summary>
    
    Register at the FKI's webpage :https://fki.tic.heia-fr.ch/databases/iam-handwriting-database)
    Download the dataset from here :https://fki.tic.heia-fr.ch/databases/download-the-iam-handwriting-database
  </details>
  <details>
   <summary>
   READ2016
   </summary>
    
    wget https://zenodo.org/record/1164045/files/{Test-ICFHR-2016.tgz,Train-And-Val-ICFHR-2016.tgz}
  </details>
  <details>
   <summary>
   LAM
   </summary>
    
    Download the dataset from here: https://aimagelab.ing.unimore.it/imagelab/page.asp?IdPage=46
  </details>

---

## Training

To reproduce the main HTR-MB configuration, run:

```bash
sh run/train.sh
```

The default configuration uses:

```text
Architecture : Bidirectional Mamba
Depth        : 4 BDM blocks
Head         : 2-layer BiLSTM
Input size   : 512 × 64
Batch size   : 128
Optimizer    : AdamW
Learning rate: 1e-3
Iterations   : 100,000
```

Individual parameters can also be configured directly through `train.sh`.

---

## Repository Structure

```text
HTR-MB/
├── data/          # Dataset utilities and configuration
├── model/         # HTR-MB architecture
├── run/           # Training scripts
├── utils/         # Utility functions
├── img/           # README assets
├── train.py       # Training
├── valid.py       # Validation
└── test.py        # Evaluation
```

---

## Citation

If you find this repository useful for your research, please cite our paper:

```bibtex
@article{BOTTI2027114858,
title = {HTR-MB: Combining Mamba and BiLSTM for Handwritten Text Recognition},
journal = {Pattern Recognition},
volume = {182},
pages = {114858},
year = {2027},
issn = {0031-3203},
doi = {https://doi.org/10.1016/j.patcog.2026.114858},
url = {https://www.sciencedirect.com/science/article/pii/S0031320326018224},
author = {Filippo Botti and Vittorio Bernuzzi and Tomaso Fontanini and Massimo Bertozzi and Andrea Prati},
}
```

---

## Acknowledgements

This implementation builds upon ideas and publicly available code from [HTR-VT](https://github.com/Intellindust-AI-Lab/HTR-VT), [VAN](https://github.com/FactoDeepLearning/VerticalAttentionOCR) and [OrigamiNet](https://github.com/IntuitionMachines/OrigamiNet).  

## License

Please refer to the repository license for usage and distribution terms.