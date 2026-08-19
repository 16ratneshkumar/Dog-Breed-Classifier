# 🐶 Dog Breed Classifier

A Python-based image classification project that uses **pretrained CNN models** (ResNet, AlexNet, VGG) to classify pet images, identify dog breeds, and compare model performance.

This project is part of the **Udacity AI Programming with Python Nanodegree** (AIPND).

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Models Used](#models-used)
- [Results](#results)
- [Installation](#installation)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [File Descriptions](#file-descriptions)
- [License](#license)

---

## Overview

This project compares three pretrained CNN architectures on the task of:
1. Correctly identifying whether a pet image is a **dog or not a dog**
2. Correctly classifying the **dog breed** for dog images
3. Measuring overall **label match** between true labels (from filenames) and classifier output

Pet image labels are extracted from filenames (e.g., `Boston_terrier_02259.jpg` → `boston terrier`) and compared against the CNN classifier's predictions.

---

## Project Structure

```
Dog-Breed-Classifier/
│
├── check_images.py                  # Main entry point
├── classifier.py                    # CNN classifier using PyTorch pretrained models
├── get_input_args.py                # Parses command-line arguments
├── get_pet_labels.py                # Extracts pet labels from image filenames
├── classify_images.py               # Classifies images using the CNN model
├── adjust_results4_isadog.py        # Adjusts results to check dog vs not-dog
├── calculates_results_stats.py      # Computes performance statistics
├── print_results.py                 # Prints final results and summary
│
├── dognames.txt                     # List of valid dog breed names
├── imagenet1000_clsid_to_human.txt  # ImageNet class-to-label mapping
│
├── pet_images/                      # Folder with 40 pet images for classification
├── uploaded_images/                 # Folder for custom uploaded images
│
├── run_models_batch.sh              # Script to run all 3 models on pet_images/
├── run_models_batch_uploaded.sh     # Script to run all 3 models on uploaded_images/
│
├── resnet_pet-images.txt            # ResNet results on pet_images/
├── alexnet_pet-images.txt           # AlexNet results on pet_images/
├── vgg_pet-images.txt               # VGG results on pet_images/
│
├── resnet_uploaded-images.txt       # ResNet results on uploaded_images/
├── alexnet_uploaded-images.txt      # AlexNet results on uploaded_images/
├── vgg_uploaded-images.txt          # VGG results on uploaded_images/
│
├── requirements.txt                 # Python dependencies
└── .gitignore
```

---

## Models Used

| Model        | Architecture       | Description                     |
|--------------|--------------------|---------------------------------|
| **VGG16**    | Very Deep CNN      | 16 weight layers, high accuracy |
| **ResNet18** | Residual Network   | Skip connections, 18 layers     |
| **AlexNet**  | Classic Deep CNN   | Pioneering architecture, fast   |

All models are **pretrained on ImageNet** and used for inference (no fine-tuning).

---

## Results

Results are from running all three models on the 40 images in `pet_images/`.

| Metric                       | ResNet | AlexNet | VGG    |
|------------------------------|--------|---------|--------|
| **% Correct Dogs**           | 100.0% | 100.0%  | 100.0% |
| **% Correct Not-Dogs**       | 90.0%  | 100.0%  | 100.0% |
| **% Correct Breed**          | 90.0%  | 80.0%   | 93.3%  |
| **% Label Match (Overall)**  | 82.5%  | 75.0%   | 87.5%  |

> ✅ **VGG16** performs best overall — highest breed accuracy (93.3%) and perfect not-dog classification (100%).

---

## Installation

### Prerequisites

- Python 3.6+
- pip

### Clone the Repository

```bash
git clone https://github.com/16ratneshkumar/Dog-Breed-Classifier.git
cd Dog-Breed-Classifier
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

> **Note:** `torchvision` installs `torch` (PyTorch) as a dependency automatically.

---

## Usage

### Run a Single Model

```bash
python check_images.py --dir pet_images/ --arch vgg --dogfile dognames.txt
```

**Arguments:**

| Argument    | Default        | Description                                   |
|-------------|----------------|-----------------------------------------------|
| `--dir`     | `pet_images/`  | Path to the folder containing images          |
| `--arch`    | `vgg`          | CNN model: `vgg`, `resnet`, or `alexnet`      |
| `--dogfile` | `dognames.txt` | Path to the text file containing dog names    |

### Run All Three Models (Batch)

```bash
# For pet_images/
sh run_models_batch.sh

# For your own uploaded images
sh run_models_batch_uploaded.sh
```

Results are saved automatically to text files: `resnet_pet-images.txt`, `alexnet_pet-images.txt`, `vgg_pet-images.txt`.

### Classify Your Own Images

1. Place `.jpg` images in the `uploaded_images/` folder.
2. Run:
   ```bash
   sh run_models_batch_uploaded.sh
   ```
3. Results will be written to `resnet_uploaded-images.txt`, `alexnet_uploaded-images.txt`, and `vgg_uploaded-images.txt`.

---

## How It Works

```
┌──────────────────────────────────────────────────────────────┐
│                        check_images.py                        │
│                                                              │
│  1. Parse CLI args  (--dir, --arch, --dogfile)               │
│  2. Extract pet labels from image filenames                   │
│  3. Run pretrained CNN to classify each image                 │
│  4. Mark results as dog / not-dog using dognames.txt          │
│  5. Calculate statistics (% correct dogs, breeds, etc.)       │
│  6. Print summary results                                     │
└──────────────────────────────────────────────────────────────┘
```

**Label Extraction Example:**
```
Filename:  Boston_terrier_02259.jpg
           ↓  lowercase + split on '_' + keep alpha words
Pet Label: "boston terrier"
```

**Classifier Output Example:**
```
Classifier: "Boston bull, Boston terrier"
→ Matched against pet label → MATCH ✅
```

---

## File Descriptions

| File | Description |
|------|-------------|
| `check_images.py` | Main program; orchestrates the full pipeline |
| `classifier.py` | Loads pretrained models (VGG16, ResNet18, AlexNet) and classifies images |
| `get_input_args.py` | Uses `argparse` to parse `--dir`, `--arch`, `--dogfile` |
| `get_pet_labels.py` | Extracts true pet labels from image filenames |
| `classify_images.py` | Runs each image through the chosen CNN model |
| `adjust_results4_isadog.py` | Checks if the pet label and classifier label are dogs |
| `calculates_results_stats.py` | Computes counts and percentages from results dictionary |
| `print_results.py` | Prints summary stats and optionally misclassified cases |
| `dognames.txt` | Reference list of dog breed names |
| `imagenet1000_clsid_to_human.txt` | ImageNet class index to human-readable label map |

---

## License

This project is based on the **Udacity AI Programming with Python Nanodegree** starter code and is intended for educational purposes.

---

<p align="center">Made with ❤️ by <a href="https://github.com/16ratneshkumar">Ratnesh Kumar</a></p>
