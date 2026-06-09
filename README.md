# DecodeOCR — AI Image & Text Recognition Pipeline

**Project 4: Image or Text Recognition (Basic)**  
Decode Labs AI Engineer Intern — Mastery Phase

A Python-based OCR pipeline that transforms raw visual data into machine-readable text. Implements the full IPO (Input → Process → Output) model: image pre-processing via OpenCV, text recognition via Tesseract, and confidence-based output filtering.

## Requirements

- Python 3.10+
- [Tesseract OCR](https://github.com/UB-Mannheim/tesseract/wiki) engine installed on your system
- Python packages (install via below)

## Setup

```bash
# 1. Create and activate virtual environment
python -m venv .venv
.venv\Scripts\Activate.ps1     # Windows
# source .venv/bin/activate    # macOS/Linux

# 2. Install Python dependencies
pip install -r requirements.txt

# 3. Install Tesseract OCR engine
# Windows: winget install UB-Mannheim.TesseractOCR
# macOS:   brew install tesseract
# Linux:   sudo apt install tesseract-ocr
```

If Tesseract is installed to a non-default path, update the path in `recognizer.py`:

```python
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

## Usage

```bash
# Run with built-in sample image (no arguments needed)
python recognizer.py

# Run on your own image
python recognizer.py --image path/to/your_image.png

# Full pipeline with debug images and exports
python recognizer.py --image scan.png --debug --export text.txt --csv data.csv

# Force a specific PSM mode instead of auto-scan
python recognizer.py --psm 6

# Adjust confidence threshold (default: 80%)
python recognizer.py --threshold 70
```

### Options

| Flag              | Description                                             |
| ----------------- | ------------------------------------------------------- |
| `--image, -i`     | Path to input image (default: generates a sample image) |
| `--psm`           | Force a specific Tesseract PSM mode                     |
| `--threshold, -t` | Minimum confidence % to include a word (default: 80)    |
| `--debug, -d`     | Save pre-processing stage images to `output/`           |
| `--export, -e`    | Export extracted text to a file                         |
| `--csv`           | Export per-word confidence data to CSV                  |
| `--no-auto-psm`   | Skip PSM auto-scan; use PSM 3 only                      |

## Pipeline Stages

1. **Sample Image Generation** — Creates a test image with known text on a gradient+noise background for standalone demos
2. **Pre-Processing** — Grayscale conversion → Gaussian blur → Otsu adaptive thresholding
3. **PSM Auto-Scan** — Tests multiple Page Segmentation Modes (3, 6, 7, 13), selects the highest-confidence result
4. **OCR Recognition** — Extracts text via Tesseract with per-word confidence scoring
5. **Confidence Filtering** — Rejects words below the 80% threshold
6. **Output** — Annotated image with bounding boxes, confidence report, text export, CSV export

## Project Structure

```
P4/
├── recognizer.py          # OCR pipeline (entry point)
├── requirements.txt       # Python dependencies
├── README.md              # This file
├── .venv/                 # Virtual environment (local)
├── sample_input.png       # Generated sample image (on first run)
└── output/                # Debug and annotated images (on run)
    ├── debug_01_grayscale.png
    ├── debug_02_blurred.png
    ├── debug_03_otsu.png
    └── annotated_result.png
```
