# Omicimage

**Omicimage** introduces a machine-learning–native representation of genomic data by unifying sequencing alignments (SAM format) and optional CpG methylation information into structured RGBA images. In this representation, each pixel encodes base identity, methylation state, strand orientation, and coverage within a shared genomic coordinate system, directly addressing structural incompatibilities between standard genomics formats and modern machine-learning workflows.

This design enables genomic and epigenomic data to be treated as a single, coherent input for machine-learning models, while remaining interpretable and suitable for visualisation and downstream computational analysis.

---

## Features

- Machine-learning–native representation of genomic alignments
- Unified encoding of sequence and CpG methylation data within a shared coordinate system
- Conversion of SAM alignment data into structured RGBA image format
- Optional integration of CpG methylation percentage data
- Produces the following outputs:
  - **`image.png`** — RGBA image encoding genomic and epigenomic signals
  - **`readalignmentcoordinates.tsv`** — per-pixel genomic metadata
  - **`metadata`** — image dimensions and encoding parameters
- Two command-line entry points:
  - `build-rgba-nomethylation`
  - `build-rgba-methylation`
- Python API for programmatic use and pipeline integration

---

## Installation

Requires **Python 3.9 or later**.

```bash
pip install omicimage
```

Dependencies (installed automatically via pip):
- `numpy`
- `pillow`
- `pysam`
- `tqdm`

---

## Command-Line Usage

### 1. No methylation data
```bash
build-rgba-nomethylation \
  my_reads.sam \
  results/run1 \
  --width 2500 \
  --full-read \
  --mapq 20
```

### 2. With methylation data
```bash
build-rgba-methylation \
  my_reads.sam \
  my_sample.cpg.txt \
  results/run2 \
  --width 2500 \
  --full-read \
  --mapq 20
```

### Arguments

**Positional:**
- `sam` — input SAM alignment file
- `output_prefix` — prefix for the output directory (a timestamp is appended)
- `cpg_report` — CpG methylation report (required only for methylation mode)

**Optional:**
- `--width` — image width in pixels (default: 2500 pixels)
- `--full-read` — encode all bases from each read rather than only the first base
- `--mapq` — minimum mapping quality (MAPQ) threshold for read filtering

---

## Output Structure

For example, with an `output_prefix` set to `results/run1`, the following directory is created:

```
results/run1-YYYY-MM-DD_HH-MM-SS/
├── image.png
├── readalignmentcoordinates.tsv
└── metadata
```

---

## Input Format Notes

- **SAM file** — Any valid SAM file is supported (sorted or unsorted).
- **CpG methylation report** — Tab-delimited text files with at least:
  ```
  chromosome  position  ...  methylation_percentage
  ```

---

## Python API

```python
from omicimage.image_encoder import create_rgba_array, convert_rgba_array_to_image
from omicimage.sam_parser import open_sam_file
from omicimage import __version__

print(__version__)
```

---

## Development

Clone the repo:
```bash
git clone https://github.com/Abraham-Gabriel/omicimage.git
cd omicimage
```

Install for local development:
```bash
pip install -e .
```

Run tests (if applicable):
```bash
pytest
```

---

## License

MIT License — see `LICENSE`