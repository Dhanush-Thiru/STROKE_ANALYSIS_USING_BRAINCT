# NeuroGPU — GPU-Accelerated Brain CT Analysis

NeuroGPU is a local, research-only prototype for training and serving a three-class classifier on the [Brain Stroke CT Dataset](https://www.kaggle.com/datasets/ozguraslank/brain-stroke-ct-dataset). It makes no clinical diagnosis, does not rule out stroke, does not store uploads, and must not be used for treatment decisions.

## What is included

- Dataset inspector with corrupt-file, dimensions, hash-duplicate, possible overlay, class-mapping, and group-ID reports.
- Reproducible split manifests with explicit leakage limitations.
- PyTorch ResNet-18 baseline, checkpoint metadata, CPU/CUDA selection, evaluation, and fair warmed-up benchmark exports.
- FastAPI inference API and a responsive React dashboard that never invents results.

## Setup (Windows PowerShell)

```powershell
cd neurogpu
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If PowerShell blocks activation, use `Set-ExecutionPolicy -Scope Process Bypass` in your own terminal session or run `.venv\Scripts\python.exe` explicitly. For CUDA, install the PyTorch build matching your NVIDIA driver from the official PyTorch selector; CPU PyTorch is fully supported.

## Get data

Authenticate Kaggle CLI using your own account (never put a Kaggle token in this repository), then run:

```powershell
kaggle datasets download -d ozguraslank/brain-stroke-ct-dataset -p data/raw --unzip
```

Alternatively download it manually and extract it below `data/raw/`. Do not assume its directory layout: inspect it first.
## Get data

Authenticate Kaggle CLI using your own account (never put a Kaggle token in this repository), then run:

```powershell
kaggle datasets download -d ozguraslank/brain-stroke-ct-dataset -p data/raw --unzip
```

Alternatively download it manually and extract it below `data/raw/`. Do not assume its directory layout: inspect it first.

## Reproducible workflow

```powershell
python ml/inspect_dataset.py --data-root data/raw --output-dir outputs/dataset_report
python ml/prepare_dataset.py --data-root data/raw --output-dir data/processed
python ml/train.py --config ml/configs/default.yaml --data-root data/raw
python ml/evaluate.py --checkpoint models/best_model.pt --data-root data/raw --split test
python ml/benchmark.py --checkpoint models/best_model.pt --data-root data/raw
```

The inspector stops if it cannot establish exactly `Bleeding`, `Ischemia`, and `Normal` safely. Examine its reports and adapt only after verifying the actual dataset. Training downloads pretrained weights only if they are not already cached; set `pretrained: false` in the YAML for offline training from scratch.

## Run the product

Terminal 1:

```powershell
python -m uvicorn backend.app.main:app --app-dir . --reload
```

Terminal 2:

```powershell
cd frontend
npm.cmd install
npm.cmd run dev
```

Open the displayed Vite URL. Set `NEUROGPU_CHECKPOINT` to point at a different model if required. API docs are at `http://localhost:8000/docs`.

## Test

```powershell
python -m pytest backend/tests -q
```

Basic tests use synthetic images and need neither Kaggle data nor a GPU. See [methodology](docs/methodology.md) and [evaluation](docs/evaluation.md) for constraints. Linux/macOS activation is `source .venv/bin/activate`; the other commands are unchanged.
=======
# STROKE_ANALYSIS_USING_BRAINCT
research-focused web application for experimental brain CT image classification. It uses a trained ResNet-18 deep-learning model to categorize CT PNG images into the dataset labels Bleeding, Ischemia, or Normal.
>>>>>>> ee2120da5042c9fdd85fcb97caa842399a7b3ca0
