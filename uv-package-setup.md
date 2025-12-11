# UV Package Manager Installation & Project Setup

## What is UV Package Manager?

UV is an extremely fast Python package and project manager written in Rust. It is designed to replace pip, pip-tools, and other package managers with significantly better performance.

### Key Features:
- **10-100x faster than pip** for package installation and management
- Single tool to replace pip, pip-tools, and related tools
- Comprehensive project management with universal locker file
- Can run scripts and install multiple Python versions
- Supports running tools published as Python packages

## Installation Steps

### Step 1: Install UV Package Manager

**For macOS and Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**For Windows (PowerShell):**
```powershell
powershell -Command "irm https://astral.sh/uv/install.ps1 | iex"
```

**Alternative Method (Using pip):**
```bash
pip install uv
```

**Verify Installation:**
```bash
uv --version
```

## Project Setup Steps

### Step 2: Create Project Workspace

Create a new folder for your project:
```bash
mkdir rag-udemy
cd rag-udemy
```

### Step 3: Initialize Project with UV

Initialize the folder as a UV project workspace:
```bash
uv init
```

This command will:
- Create a project structure
- Generate a `pyproject.toml` file with project metadata
- Set up initial configuration files
- Detect your Python version (e.g., Python 3.13)

### Step 4: Create Virtual Environment

Create a new virtual environment named `.venv`:
```bash
uv venv .venv
```

If you want to use a specific Python version:
```bash
uv python install 3.13
uv venv .venv --python 3.13
```

### Step 5: Activate Virtual Environment

**On Windows (Command Prompt):**
```bash
.venv\Scripts\activate
```

**On macOS/Linux:**
```bash
source .venv/bin/activate
```

## Installing Dependencies

### Step 6: Create Requirements File

Create a `requirements.txt` file with the following dependencies for RAG implementation:

```txt
langchain
langchain-groq
faiss-cpu
tiktoken
langchain-community
langchain-openai
sentence-transformers
pypdf
python-dotenv
ipykernel
```

### Step 7: Install Dependencies using UV

Install all packages from requirements file:
```bash
uv add -r requirements.txt
```

Or install packages individually:
```bash
uv add langchain langchain-groq faiss-cpu tiktoken langchain-community langchain-openai sentence-transformers pypdf python-dotenv ipykernel
```

## Quick Comparison: UV vs pip

| Aspect | UV | pip |
|--------|-----|-----|
| **Speed** | 10-100x faster | Standard |
| **Project Management** | Comprehensive | Basic |
| **Lock File** | Universal locker file | requirements.txt |
| **Installation Time** | Minimal | Slower |
| **Tool Replacement** | Replaces pip, pip-tools, etc. | Single tool |

## Important Notes

- **Use UV instead of pip** in this project workflow
- Always create a new virtual environment for each project
- Save your `requirements.txt` and `pyproject.toml` files
- Enable autosave in your IDE for convenience
- Verify installation by importing libraries in your notebook

## Setting up Jupyter Notebook

Install IPykernel for Jupyter notebook support:
```bash
uv add ipykernel
```

Then create a `.ipynb` file and select the kernel from your virtual environment.

## Verification

Test your setup by running a quick Python script:
```bash
uv run python -c "import langchain; print('Success')"
```

Or in an interactive environment:
```python
import langchain
print("Installation successful!")
```

---

This setup provides a fast, modern Python development environment specifically configured for RAG (Retrieval-Augmented Generation) projects using LangChain and related libraries.
