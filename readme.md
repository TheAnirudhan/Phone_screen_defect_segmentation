**Screen Scratch Segmentation with Detectron2**

## Overview

This project implements an image segmentation pipeline using Detectron2 to detect scratches on device screens from top-down images. The model is fine-tuned from a pre-trained COCO instance segmentation network (`mask_rcnn_R_50_FPN_3x`). It covers data registration, training, inference, and evaluation.

## Repository Structure

```
├── Datasets/
│   ├── mobileScratches/
│   │   ├── training/
│   │   │   ├── images/
│   │   │   └── training_ann.json
│   │   └── testing/
│   │       ├── images/
│   │       └── testing_ann.json
├── models/
│   └── Detectron2_Models/
│       ├── config.yaml
│       └── model_final.pth
├── README.md                      
└── MobileScratchSegmentation.ipynb     
└── requirements.txt                
└── sample_image.jpg
└── street_small.jpg                
```

## Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/yourusername/screen-scratch-segmentation.git
   cd screen-scratch-segmentation
   ```

2. **Create a Python environment** (recommended):

   ```bash
   python -m venv venv
   source venv/bin/activate        # Linux/Mac
   venv\\Scripts\\activate       # Windows
   ```

3. **Install dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

4. **Install Detectron2** (ensure CUDA compatibility):

   ```bash
   python -m pip install detectron2 -f \\
     https://dl.fbaipublicfiles.com/detectron2/wheels/cu113/torch1.10/index.html
   ```

## Configuration

* `\*\*Edit \*\*\` under `models/Detectron2_Models/` to adjust dataset paths, batch size, learning rate, and number of classes.
* **Dataset registration** is in `train.py` via `register_coco_instances"my_dataset_{train,val}"` pointing to your JSON and image folders.

## Training

Run the training script:

```bash
python scripts/train.py \
  --config-file models/Detectron2_Models/config.yaml \
  --output models/Detectron2_Models
```

* Outputs (model weights, final config) are saved under `models/Detectron2_Models/`.

## Inference & Evaluation

To perform inference on validation images and evaluate:

```bash
python scripts/inference.py \
  --config-file models/Detectron2_Models/config.yaml \
  --weights models/Detectron2_Models/model_final.pth
```

* Visual results are displayed with color-coded masks.
* COCO metrics (mAP, mAR) are printed to the console.

## Scope & Applications

* **Manufacturing QA**: Automated scratch detection on smartphone and tablet assembly lines using top-down camera setups.
* **Electronics Assembly**: Inline quality checks for screens in laptops, monitors, and automotive infotainment panels.
* **Hardware Testing Labs**: Batch inspection of display components before final packaging.

## Drawbacks & Limitations & Limitations

* **Data Dependency**: Performance relies on annotated quality and quantity; small datasets may lead to overfitting.
* **Inference Speed**: Mask R-CNN can be slow on CPU-only setups; real-time demands require optimized hardware or lightweight architectures.
* **Generalization**: Model trained on one device type may not transfer to screens with different textures or lighting conditions without retraining.

## Personal Technical Constraints

* **Hardware**: Developed and tested on a personal laptop with limited GPU memory (e.g., 6–8 GB), requiring reduced batch sizes and shorter training runs.
* **Dataset Size**: Used a custom dataset of a few hundred annotated images, which may limit edge-case coverage.
* **Development Time**: Balanced project work alongside coursework and personal commitments, resulting in prioritized experiments and fewer hyperparameter sweeps.
