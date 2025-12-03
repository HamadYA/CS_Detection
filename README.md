# CS_Detection

Automated detection of **cytoplasmic strings** in human embryo
time-lapse videos using deep learning.\
This project provides a reproducible pipeline to process embryo imaging
data and identify cytoplasmic strings that may support embryologists in
embryo-quality assessment.

------------------------------------------------------------------------

## 🔍 Overview

Cytoplasmic strings (CS) are structural patterns observed in developing
embryos. Detecting them consistently from time-lapse videos is
challenging and time-intensive.\
**CS_Detection** uses neural-network based models to detect these
structures automatically, offering:

-   Automated frame-by-frame CS detection\
-   Easy-to-run scripts and notebooks\
-   Modular and extendable deep-learning pipeline\
-   Support for future classification or downstream analysis

------------------------------------------------------------------------

## 📁 Repository Structure

    CS_Detection/
    ├── detection/           # Main detection models, scripts, and utilities
    ├── classification/      # Optional classification experiments (if used)
    ├── notebooks/           # Jupyter notebooks for exploration and visualization
    ├── data/                # User-provided embryo video data (not included in repo)
    ├── requirements.txt     # Python dependencies
    └── README.md

------------------------------------------------------------------------

## ⚙️ Installation

1.  Clone the repository:

``` bash
git clone https://github.com/HamadYA/CS_Detection.git
cd CS_Detection
```

2.  Install dependencies:

``` bash
pip install -r requirements.txt
```

------------------------------------------------------------------------

## 🚀 Usage

### **1. Prepare your embryo time-lapse video data**

Place your video/frame sequences into the `data/` directory, or adjust
paths in the detection scripts.

### **2. Run the detection pipeline**

Run directly via script:

``` bash
python detection/run_detection.py
```

Or use the provided Jupyter notebooks:

``` bash
jupyter notebook notebooks/
```

### **3. View the results**

Detections are saved to the output directory defined in the script.

------------------------------------------------------------------------

## 🤝 Contributing

1.  Fork the repo\
2.  Create a feature branch\
3.  Commit your changes\
4.  Open a pull request

------------------------------------------------------------------------

## 👤 Author

**HamadYA**\
GitHub: https://github.com/HamadYA

------------------------------------------------------------------------

## 📄 License

Specify your license here.
