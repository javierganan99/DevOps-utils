# Managing Multiple Versions of a Python Package using Poetry

This document explains how to manage different sets of dependencies (such as common, development, and deployment) in Python packages using `pyproject.toml` with Poetry. It also covers how to install these versions using both Poetry and pip.

## 1. Setting Up `pyproject.toml`

The `pyproject.toml` file is the core configuration file for Poetry projects. You can manage multiple dependency groups (e.g., common, development, and deployment) by organizing them in specific sections of the `pyproject.toml` file.

Here’s how you can structure it:

### Common Dependencies

Common dependencies are the ones required in all cases (for normal package usage). These dependencies are listed under `[tool.poetry.dependencies]`.

```toml
[tool.poetry.dependencies]
python = "^3.8"
common-dependency-1 = "^1.0"
common-dependency-2 = "^2.0"
```

### Development Dependencies

Development dependencies include tools that are required for testing, formatting, or other development purposes. These dependencies are usually listed under `[tool.poetry.dev-dependencies]` for easy management.

```toml
[tool.poetry.dev-dependencies]
pytest = "^6.0"
black = "^22.0"
```

To group development dependencies in an "extra" (so they can be optionally installed), you can also add them under `[tool.poetry.extras]`:

```toml
[tool.poetry.extras]
development = ["pytest", "black"]
```

### Deployment Dependencies

Deployment dependencies are required for production environments, or when you want to prepare the package for deployment. These are defined under the `[tool.poetry.extras]` section as well:

```toml
[tool.poetry.extras]
deployment = ["deployment-dependency-1", "deployment-dependency-2"]
```

### Full `pyproject.toml` Example

```toml
[tool.poetry]
name = "your-package-name"
version = "0.1.0"
description = "Your poetry package"
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.8"
# Common dependencies
common-dependency-1 = "^1.0"
common-dependency-2 = "^2.0"

[tool.poetry.dev-dependencies]
# Development-specific dependencies
pytest = "^6.0"
black = "^22.0"

[tool.poetry.extras]
# Define extras for deployment and development
deployment = ["deployment-dependency-1==1.2.3", "deployment-dependency-2==4.5.6"]
development = ["pytest", "black"]
```

## 2. Installing the Package

### Common Installation (Normal Usage)

To install the package with only the common dependencies (the default installation), you can use the following commands:

#### Using Poetry

```bash
poetry install
```

#### Using pip

```bash
pip install -e .
```

This will install only the common dependencies listed under `[tool.poetry.dependencies]`.

### Development Installation

To install both the common and development dependencies, you need to specify the `development` extra.

#### Using Poetry

```bash
poetry install --with development
```

This command will install both the common dependencies and the development dependencies defined in the `[tool.poetry.dev-dependencies]` and the `development` extra.

#### Using pip

To install the package with pip and include the development dependencies:

```bash
pip install -e .[development]
```

This will install the common dependencies as well as the development dependencies specified in the `development` extra.

### Deployment Installation

If you need the package with deployment-specific dependencies, use the `deployment` extra.

#### Using Poetry

```bash
poetry install --with deployment
```

This will install both the common dependencies and the deployment dependencies defined in the `deployment` extra.

#### Using pip

To install with pip and include the deployment dependencies:

```bash
pip install .[deployment]
```

This will install the common dependencies as well as the deployment dependencies specified in the `deployment` extra.

### Full Installation (Development + Deployment)

If you need both development and deployment dependencies, you can install them together:

#### Using Poetry

```bash
poetry install --with development --with deployment
```

#### Using pip

You can use pip to install both extras simultaneously:

```bash
pip install .[development,deployment]
```

## 3. Summary of Installation Options

| Use Case                     | Poetry Command                                        | pip Command                             |
| ---------------------------- | ----------------------------------------------------- | --------------------------------------- |
| **Common Installation**      | `poetry install`                                      | `pip install -e .`                      |
| **Development Installation** | `poetry install --with development`                   | `pip install -e .[development]`         |
| **Deployment Installation**  | `poetry install --with deployment`                    | `pip install .[deployment]`             |
| **Development + Deployment** | `poetry install --with development --with deployment` | `pip install .[development,deployment]` |

Using extras in `pyproject.toml` allows you to define different "sets" of dependencies that can be installed only when needed. This is useful for keeping your package lightweight when users don't require all dependencies, and for separating the dependencies needed for development or deployment.
