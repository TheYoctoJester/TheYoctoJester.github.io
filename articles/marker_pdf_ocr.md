# Extracting Text from Scanned PDFs with Marker

*January 6, 2026*

I recently needed to extract text from a 30 MB scanned book PDF (German language). After some research, I settled on [marker-pdf](https://github.com/datalab-to/marker), an ML-based tool that converts PDFs directly to Markdown with good structure preservation. This post documents the setup process on both macOS and Windows with NVIDIA GPU acceleration.

## Why Marker?

For scanned documents, you need OCR (Optical Character Recognition). The two main local approaches are:

1. **Tesseract pipeline** (traditional OCR) — Mature, lightweight, but outputs plain text without structure. Markdown formatting requires post-processing.

2. **Marker** (ML-based) — Uses deep learning models for layout detection and text recognition. Outputs Markdown with preserved headings, paragraphs, lists, and tables. Heavier dependencies but better results for structured documents.

Marker supports 90+ languages (including German), extracts images, and can optionally use an LLM to improve accuracy.

## macOS Setup

### Prerequisites

Marker requires Python 3.10-3.12. Python 3.13+ is too new and will cause compatibility issues with dependencies.

If you're using Homebrew and have a newer Python as default:

```bash
# Install Python 3.12 (won't affect your default python3)
brew install python@3.12

# Create venv using that specific version
/opt/homebrew/opt/python@3.12/bin/python3 -m venv ~/marker-env
source ~/marker-env/bin/activate

# Verify version
python --version
```

### Installation

```bash
pip install marker-pdf
```

The first run will download several GB of ML models.

### Basic Usage

```bash
marker_single /path/to/your/book.pdf --output_dir ./output --force_ocr
```

Key flags:
- `--force_ocr` — Forces OCR on all pages. Essential for scanned documents.
- `--output_dir` — Where to save results.
- `--page_range "0-10"` — Process only specific pages (useful for testing).
- `--output_format json` — Output JSON instead of Markdown.
- `--disable_image_extraction` — Skip image extraction entirely.

Note: On macOS without NVIDIA GPU, processing runs on CPU (or MPS on Apple Silicon). Expect slower performance compared to CUDA-accelerated systems.

## Windows 11 Setup with NVIDIA GPU

### Prerequisites

1. Install Python 3.12 from [python.org](https://python.org) (not the Microsoft Store version)
2. Check "Add to PATH" during installation
3. Note your CUDA version: run `nvidia-smi` in cmd

### Installation (Order Matters!)

The installation order is important. Install PyTorch with CUDA *before* marker-pdf to avoid dependency conflicts.

```cmd
:: Create and activate venv
python -m venv C:\marker-env
C:\marker-env\Scripts\activate.bat
```

> **PowerShell Note:** If using PowerShell and you get an execution policy error, either:
> - Use `cmd.exe` with `activate.bat` instead, or
> - Run as Admin: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

```cmd
:: Install PyTorch with CUDA FIRST (for CUDA 12.x)
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121

:: Then install marker
pip install marker-pdf
```

### Fixing Dependency Conflicts

Marker's installation may downgrade torch or cause version mismatches. After installation, check:

```cmd
python -c "import torch; print(torch.__version__, torch.cuda.is_available())"
```

If CUDA is not available (`False`), reinstall PyTorch:

```cmd
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121 --force-reinstall
```

You may also need to pin Pillow to a compatible version:

```cmd
pip install "pillow<11.0.0"
```

Dependency warnings about version mismatches are often not fatal—test the actual conversion before troubleshooting further.

### Verify GPU Acceleration

```cmd
python -c "import torch; print(torch.__version__, torch.cuda.is_available())"
```

Expected output: `2.5.1+cu121 True` (or similar with `True` for CUDA)

### Usage

```cmd
marker_single "C:\path\to\book.pdf" --output_dir .\output --force_ocr
```

## Output Structure

Marker creates a directory containing:
- `book.md` — The Markdown output
- `images/` — Extracted figures, referenced in the Markdown

## Handling Extracted Images

Marker extracts all images, including decorative elements like headers, footers, and separators. Options to clean this up:

### Option 1: Disable Image Extraction

```bash
marker_single ... --disable_image_extraction
```

### Option 2: Post-Process by Size/Aspect Ratio

Decorative elements tend to be small or have extreme aspect ratios. Simple cleanup:

```bash
# Delete images smaller than 10KB
find ./output -name "*.png" -size -10k -delete
```

More sophisticated filtering (Python):

```python
from pathlib import Path
from PIL import Image

output_dir = Path("./output")
for img_path in output_dir.rglob("*.png"):
    with Image.open(img_path) as img:
        w, h = img.size
        aspect = max(w, h) / min(w, h) if min(w, h) > 0 else 999
        area = w * h
        
        # Remove if: tiny, or extremely wide/tall (likely decorative)
        if area < 5000 or aspect > 8:
            img_path.unlink()
            print(f"Removed: {img_path.name}")
```

Tune thresholds based on your specific document.

## Common Issues

| Problem | Solution |
|---------|----------|
| Python 3.13+ compatibility errors | Use Python 3.10-3.12 |
| `--langs` flag not recognized | Removed in recent versions; language detection is automatic |
| CUDA not available after install | Reinstall torch with `--force-reinstall` from CUDA index |
| Pillow version conflict | `pip install "pillow<11.0.0"` |
| PowerShell blocks activate script | Use cmd.exe or change execution policy |
| Out of memory | Use `--page_range` to process in chunks |

## Advanced Options

- `--use_llm` — Use an LLM (Gemini by default) to improve accuracy. Requires API key.
- `--paginate_output` — Add page markers to output.
- `--debug` — Save debug images showing detected layout.

See `marker_single --help` for all options.

## Performance Notes

- **GPU vs CPU:** Marker is significantly faster with CUDA. A 250-page document might take ~15 seconds on H100, but much longer on CPU.
- **Memory:** Expect 8+ GB RAM usage for large documents.
- **Model Download:** First run downloads ~2-4 GB of models.

## Resources

- [Marker GitHub Repository](https://github.com/datalab-to/marker)
- [Surya OCR (underlying engine)](https://github.com/VikParuchuri/surya)
- [Supported Languages](https://github.com/VikParuchuri/surya/blob/master/surya/recognition/languages.py)

---

*This article was drafted with assistance from Claude.*
