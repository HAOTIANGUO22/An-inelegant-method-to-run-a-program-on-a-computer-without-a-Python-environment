# An Inelegant Method to Run a Python Program Without a Pre-installed Environment

This repository demonstrates a workaround for running Python programs on computers that **do not have Python installed** or any dependencies preconfigured.

---

## Table of Contents

- [Background](#background)
- [Requirements](#requirements)
- [Methods](#methods)
  - [Plan A: Python Embeddable](#plan-a-python-embeddable)
  - [Plan B: Portable Miniconda](#plan-b-portable-miniconda)
- [Folder Structure](#folder-structure)
- [Startup Script](#startup-script)
- [Usage](#usage)
- [Flow Diagram](#flow-diagram)
- [Notes](#notes)

---

## Background

When packaging Python programs into `.exe` using tools like PyInstaller, including additional model files often requires manually adding `--add-data`, which is inconvenient.  
To simplify deployment, I explored **portable solutions** that bundle the Python interpreter, dependencies, and model files into one folder, so users can run the program with a single click.

---

## Requirements

1. **Packaging Issue**  
   - Avoid adding `--add-data` manually when converting to `.exe`.

2. **Portable Environment**  
   - Bundle **Python + dependencies + model files** into one folder, launchable via `.bat`.

3. **Simple Usage**  
   - Users only need to **unzip** the folder and **double-click** the `.bat` file to run.

---

## Methods

### Plan A: Python Embeddable

- Bundle the **Python embeddable distribution**, libraries, and model files in one folder.  
- **Issue:** Some computers fail to run the program silently.  
- **Possible Cause:** Missing `.dll` files in the embeddable package.

---

### Plan B: Portable Miniconda

> **Recommended solution** – more reliable across different computers.

#### Steps:

1. **Create a folder** named `MyApp`.  
2. **Download** the portable version of Miniconda (lighter than Anaconda).  
3. **Unzip** Miniconda into `MyApp/conda/`.  
4. Place your existing environment into `conda/envs/`.

---

## Folder Structure
MyApp/
├─ conda/
│ ├─ Scripts/
│ ├─ Library/
│ └─ envs/
├─ models/
└─ run.bat


---

## Startup Script

Create a `run.bat` file in the root folder:

```bat
@echo off
setlocal
set ROOT=%~dp0

:: Activate the portable Miniconda environment (PinPong)
call "%ROOT%conda\Scripts\activate.bat" "%ROOT%conda\envs\PinPong"

:: Check if Python exists
if not exist "%ROOT%conda\envs\PinPong\python.exe" (
    echo Python environment not found. Please check if the PinPong environment was copied correctly to conda\envs\PinPong
    pause
    exit /b
)

:: Run the main program
"%ROOT%conda\envs\PinPong\python.exe" "%ROOT%main.py"

pause

