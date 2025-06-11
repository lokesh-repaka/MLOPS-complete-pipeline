# MLOps Complete Pipeline

This repository demonstrates a complete MLOps pipeline using DVC, Git, and Python for managing machine learning workflows, experiments, and reproducibility.

## Project Structure

- `src/` — Source code for all pipeline components (data ingestion, preprocessing, feature engineering, model building, evaluation).
- `data/` — Contains raw, interim, and processed datasets (ignored by Git).
- `models/` — Trained model artifacts (ignored by Git).
- `reports/` — Generated reports and metrics (ignored by Git).
- `logs/` — Log files for each pipeline stage.
- `experiments/` — Experiment notebooks and datasets.
- `dvclive/` — DVC Live experiment tracking outputs.
- `params.yaml` — Centralized parameters for pipeline stages.
- `dvc.yaml` — DVC pipeline definition.
- `dvc.lock` — DVC pipeline lock file.

## Pipeline Setup & Usage

### 1. Initial Setup
- Create a GitHub repository and clone it locally.
- Add the `src/` folder and all components. Run them individually to verify.
- Add `data/`, `models/`, and `reports/` to `.gitignore`.
- Commit and push changes to Git.

### 2. DVC Pipeline (Without Params)
- Create `dvc.yaml` and define pipeline stages.
- Run `dvc init` and test automation with `dvc repro`.
- Visualize the pipeline with `dvc dag`.
- Commit and push changes.

### 3. DVC Pipeline (With Params)
- Add `params.yaml` for parameter management.
- Update code to load parameters from `params.yaml` (see below for code snippet).
- Run `dvc repro` to test parameterized pipeline.
- Commit and push changes.

### 4. Experiment Tracking with DVC Live
- Install DVC Live: `pip install dvclive`
- Add DVC Live code block to your main script (see below).
- Run experiments with `dvc exp run`.
- View experiments with `dvc exp show` or use the DVC VSCode extension.
- Remove or apply experiments as needed (`dvc exp remove`, `dvc exp apply`).
- Change parameters and rerun to produce new experiments.
- Commit and push changes.

## Parameter Management Example

```python
import yaml

def load_params(params_path: str) -> dict:
    """Load parameters from a YAML file."""
    with open(params_path, 'r') as file:
        params = yaml.safe_load(file)
    return params

# Usage in main():
params = load_params('params.yaml')
test_size = params['data_ingestion']['test_size']
max_features = params['feature_engineering']['max_features']
model_params = params['model_building']
```

## DVC Live Integration Example

```python
from dvclive import Live
import yaml

# ... load_params as above ...
params = load_params('params.yaml')

with Live(save_dvc_exp=True) as live:
    live.log_metric('accuracy', accuracy_score(y_test, y_pred))
    live.log_metric('precision', precision_score(y_test, y_pred))
    live.log_metric('recall', recall_score(y_test, y_pred))
    live.log_params(params)
```

## Useful Commands

- `dvc init` — Initialize DVC in the repo
- `dvc repro` — Run the pipeline
- `dvc dag` — Visualize pipeline stages
- `dvc exp run` — Run and track experiments
- `dvc exp show` — List experiments
- `dvc exp remove <exp-name>` — Remove an experiment
- `dvc exp apply <exp-name>` — Reproduce a previous experiment

## VSCode Extensions
- [DVC Extension](https://marketplace.visualstudio.com/items?itemName=iterative.dvc) — For experiment tracking and pipeline visualization

## License

This project is licensed under the MIT License.
