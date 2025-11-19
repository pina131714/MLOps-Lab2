[![MLOps Lab 1 CI Pipeline](https://github.com/pina131714/MLOps-Lab1/actions/workflows/ci.yml/badge.svg)](https://github.com/pina131714/MLOps-Lab1/actions/workflows/ci.yml)

# MLOps-Lab1: Image Preprocessing and Prediction API

## Project Overview
This repository contains the foundational components for a machine learning project, developed as the first assignment for the MLOps course. The primary objective is to implement Continuous Integration (CI) using GitHub Actions and develop the initial application logic for image processing and prediction.
The project is structured into three main modules:
1. Core Logic (`mylib`): Python functions for image processing tasks (resize, rotate, normalize, etc.) and a placeholder prediction function.
2. Command Line Interface (`cli`): A wrapper using `click` to expose core logic functions via the terminal.
3. API (`api`): A web service built with FastAPI to expose image processing endpoints via HTTP.

## Project Structure
The project follows the structure recommended in the assignment:
```text
MLOps-Lab1/
├── .github/
│   └── workflows/
│       └── ci.yml             # GitHub Actions CI workflow
├── api/
│   ├── api.py                 # FastAPI application and endpoints
│   └── __init__.py
├── cli/
│   ├── cli.py                 # Click CLI interface
│   └── __init__.py
├── mylib/
│   ├── image_processor.py     # Core image processing logic (PIL, Numpy)
│   └── __init__.py
├── templates/
│   └── home.html              # API documentation home page
├── tests/
│   ├── test_api.py            # Tests for the FastAPI endpoints
│   ├── test_cli.py            # Tests for the CLI commands
│   ├── test_logic.py          # Unit tests for the core logic
│   └── __init__.py
├── Makefile                   # Automation scripts (install, format, lint, test)
├── pyproject.toml             # Dependency management (used by uv)
└── README.md                  # This file
```

## Setup and Installation
### Prerequisites
You must have Git and the `uv` package manager installed globally.

### Local Setup
1. Clone the Repository:
```bash
git clone [https://github.com/pina131714/MLOps-Lab1.git](https://github.com/pina131714/MLOps-Lab1.git)
cd MLOps-Lab1
```
2. Run Makefile `install` target: The `Makefile` simplifies environment setup by synchronizing the virtual environment (```.venv```) and installing all dependencies defined in ```pyproject.toml```.
```bash
make install
```
This command executes: `uv sync`

## Usage
### 1. Command Line Interface (CLI)
The CLI can be used to run any of the core logic functions directly.

| Command | Description | Example |
| :--- | :--- | :--- |
| `predict` | Gets a random class prediction. | `uv run python -m cli.cli predict tiger.jpg` |
| `info` | Prints image metadata. | `uv run python -m cli.cli info tiger.jpg` |
| `resize` | Resizes and saves the image. | `uv run python -m cli.cli resize tiger.jpg -w 100 -h 100 -o resized.jpg` |
| `grayscale` | Converts image to grayscale. | `uv run python -m cli.cli grayscale tiger.jpg -o gray.jpg` |
| `rotate` | Rotates the image by an angle. | `uv run python -m cli.cli rotate tiger.jpg 90 -o rotated.jpg` |
| `normalize` | Normalizes pixel values to [0, 1]. | `uv run python -m cli.cli normalize tiger.jpg` |

### 2. FastAPI Web API

The API runs on `uvicorn` and accepts image uploads and form data.

1.  **Start the API Server:**
    ```bash
    uv run python -m api.api
    ```

2.  **Access Documentation:**
    Open your browser to `http://0.0.0.0:8000/docs` to see the Swagger UI and interact with the endpoints.

Key Endpoints:
POST /predict: Requires a file upload; returns a random prediction.
POST /resize: Requires a file upload, width (Form), and height (Form); returns the resized image.
POST /grayscale: Requires a file upload; returns the grayscale image.


## Testing
The project includes unit and integration tests for all components (logic, CLI and API).

### Running Tests
Use the `Makefile` to run the test suite:
```bash
make test
```
This command executes: `uv run pytest tests/ -vv --cov=mylib --cov=api --cov=cli`


## Continuous Integration (CI)
The main objective of this assignment is implemented by setting up a CI pipeline using GitHub Actions.

### CI Pipeline Status

The status badge below indicates the current state of the automated tests and checks for the `main` branch.

![CI Status](https://github.com/pina131714/MLOps-Lab1/actions/workflows/ci.yml/badge.svg)

### Automated Pipeline (`ci.yml`)

The CI workflow is defined in the `.github/workflows/ci.yml` file. It is configured to run automatically on every push to the repository. The pipeline executes the `make all` target, which runs the following critical stages in order:

1.  **Install Dependencies:** `make install` (`uv sync`)
2.  **Format Check:** `make format` (Black)
3.  **Lint Check:** `make lint` (Pylint)
4.  **Run Tests:** `make test` (Pytest with Coverage)

### Makefile Commands

The `Makefile` defines the necessary automation targets used by the CI pipeline:

| Target | Command | Purpose |
| :--- | :--- | :--- |
| `make install` | `uv sync` | Installs/syncs all Python dependencies. |
| `make format` | `uv run black ...` | Runs the Black formatter on all source code. |
| `make lint` | `uv run pylint ...` | Runs Pylint for static analysis. |
| `make test` | `uv run pytest ...` | Executes all tests and generates coverage data. |
| `make refactor` | `make format lint` | Runs formatting followed by linting. |
| `make all` | `make install format lint test` | Executes the full CI sequence locally or in the pipeline. |
