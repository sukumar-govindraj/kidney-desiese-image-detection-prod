# Kidney Disease Image Detection Pipeline

A robust, production‑ready deep learning pipeline for classifying kidney disease from medical images. This project uses DVC for data and pipeline versioning, MLflow for experiment tracking, and is packaged for deployment with Docker and Flask.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Dependencies](#dependencies)
4. [Prerequisites](#prerequisites)
5. [Installation & Setup](#installation--setup)
6. [Configuration](#configuration)
7. [Pipeline Workflow (DVC)](#pipeline-workflow-dvc)
8. [Experiment Tracking (MLflow)](#experiment-tracking-mlflow)
9. [Usage](#usage)

   * [Data Ingestion](#data-ingestion)
   * [Model Preparation](#model-preparation)
   * [Model Training](#model-training)
   * [Model Evaluation](#model-evaluation)
   * [Inference & Serving](#inference--serving)
10. [Project Structure](#project-structure)
11. [Contributing](#contributing)
12. [License](#license)

---
![image](https://github.com/user-attachments/assets/776f8104-36b2-4e63-a478-4bf60894ad7c)

## Project Overview

This repository implements an end-to-end pipeline for kidney disease image classification:

* **Data ingestion:** fetch and preprocess raw medical images
* **Model development:** build and fine‑tune convolutional neural networks
* **Experiment tracking:** log parameters, metrics, and artifacts with MLflow
* **Pipeline orchestration:** manage stages (ingestion, training, evaluation) with DVC
* **Serving:** expose a REST API for inference via Flask (`app.py`)

## Features

* Full reproducibility with DVC and Git
* Experiment versioning and comparison through MLflow
* Config‑driven parameters (`params.yaml`)
* Dockerized for consistent local and cloud deployment
* Modular codebase under `src/` for easy extension

## Dependencies

* **requirements.txt:** Python libraries (TensorFlow, Pandas, NumPy, Flask, DVC, MLflow, etc.)

  ````bash
  pip install -r requirements.txt
  ``` fileciteturn1file1
  ````
* **setup.py:** package metadata and entrypoint for installation (`pip install .`) fileciteturn1file0
* **params.yaml:** central configuration for hyperparameters and file paths

## Prerequisites

* Python 3.8+
* Git & Git LFS (for large image files)
* DVC 2.x
* MLflow 1.x
* Docker (optional, for container builds)

## Installation & Setup

1. **Clone the repo**

   ```bash
   git clone https://github.com/your_org/kidney-disease-image-detection.git
   cd kidney-disease-image-detection
   ```

2. **Create & activate a virtual environment**

   ```bash
   python -m venv venv
   source venv/bin/activate    # Linux/macOS
   venv\Scripts\activate     # Windows
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

3. **Initialize DVC & fetch data**

   ```bash
   dvc init
   dvc pull       # download remote data/artifacts
   ```

4. **Set up MLflow** (optional)

   ```bash
   export MLFLOW_TRACKING_URI=<your_tracking_uri>
   export MLFLOW_TRACKING_USERNAME=<user>
   export MLFLOW_TRACKING_PASSWORD=<pass>
   ```

5. **(Optional) Docker Build & Run**

   ```bash
   # Build the Docker image
   ```

docker build -t kidney-detector .

# Run the container (exposes port 8080)

docker run -d -p 8080:8080 --name kidney-detector kidney-detector

````

## Configuration

- `params.yaml`: hyperparameters (batch size, epochs, learning rate) and dataset/model paths
- `config/`: environment‑specific YAML files (e.g., `config/config.yaml` for URIs, credentials, paths)
- `model/`: contains the trained model checkpoint (`model.h5`)
- `dvc.yaml`: defines pipeline stages, dependencies, and outputs

_Ensure these files are updated to match your environment before running pipelines._

## Pipeline Workflow (DVC)

Reproduce the full ML workflow with DVC:
```bash
# Visualize pipeline
dvc dag

# Run all stages
dvc repro

# Show tracked metrics
dvc metrics show
````

## Experiment Tracking (MLflow)

1. **Launch the MLflow UI**

   ```bash
   mlflow ui --port 5000
   ```
2. **Log experiments** in code or notebooks under `research/`
3. **View runs** at `http://localhost:5000`

## Usage

### Data Ingestion

```bash
python main.py --stage data_ingestion
```

### Model Preparation

```bash
python main.py --stage prepare_base_model
```

### Model Training

```bash
python main.py --stage model_training
```

### Model Evaluation

```bash
python main.py --stage model_evaluation
```

*Notebooks in `research/` demonstrate each stage (01\_data\_ingestion, 02\_prepare\_base\_model, etc.)*

### Inference & Serving

1. **Run the Flask server**

   ```bash
   python app.py
   ```

   The server listens on `http://0.0.0.0:8080`.

2. **Access the web UI**
   Navigate to `http://localhost:8080` in your browser. Upload an image and click **Predict**.

3. **Programmatic prediction**
   Send a POST request to `/predict` with JSON:

   ```json
   {"image": "<base64-encoded-jpeg>"}
   ```

4. **Re‑train the model**

   ```bash
   curl http://localhost:8080/train
   ```

## Project Structure

```text
.
├── .dvc/                  # DVC internals
├── config/                # Environment‑specific settings
│   └── config.yaml        # URIs, credentials, paths
├── model/                 # Trained model artifacts
│   └── model.h5           # Saved CNN checkpoint
├── research/              # Jupyter notebooks (proof‑of‑concept)
│   └── *.ipynb            # data_ingestion, base_model, training, evaluation
├── src/                   # Core Python modules
│   └── cnnClassifier/
│       ├── config/        # YAML loading and config classes
│       ├── components/    # Pipeline components (data_ingestion, model_training, etc.)
│       ├── pipeline/      # Stage scripts (stage_01_data_ingestion, …)
│       ├── utils/         # common I/O & encoding utilities
│       └── entity/        # dataclasses for pipeline configs
├── templates/             # Flask HTML templates (index.html)
├── app.py                 # Flask application (/, /predict, /train routes)
├── main.py                # CLI orchestration for DVC-based pipeline
├── params.yaml            # Hyperparameters & paths
├── dvc.yaml               # DVC pipeline definition
├── dvc.lock               # Locked pipeline state
├── requirements.txt       # Python dependencies
├── Dockerfile             # Containerization blueprint
├── setup.py               # Package setup
├── scores.json            # Evaluation metrics summary
└── README.md              # This file
```

## Contributing

1. Fork this repo.
2. Create a branch: `git checkout -b feature/your-feature`.
3. Commit and push your changes.
4. Open a Pull Request.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
