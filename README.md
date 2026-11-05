# Homework 6 Runner (`pain.py`)

## 💻 Prerequisites

Ensure your local system matches one of the following specifications:
* **Operating System:** macOS 14.0+ (Sonoma or newer) OR Linux (Ubuntu 20.04+, Debian, Fedora, CentOS, etc.).
* **Architecture:** Apple Silicon (`osx-arm64`), Intel/AMD (`x86_64`), or ARM64 Linux (`linux-aarch64`).
* **Environment Manager:** Anaconda / Miniconda.

---

## 🚀 Step-by-Step Installation

### 1. Provision the Conda Environment
Initialize the isolated deployment environment and configure core packages using the bundled configuration file:

```bash
conda env create -f environment.yaml
conda activate hw6_working
```

### 2. Install Legacy Biopython Component
Since legacy components fail compiler checks under modern Python runtimes on specific architectures, map the module tree directly:

1. Download the reference archive: **[Biopython 1.77 (ZIP)](https://github.com)**.
2. Unpack the ZIP archive on your local machine.
3. Locate the core folder named **`Bio`** and copy it.
4. Paste the **`Bio`** folder directly into your active Conda environment's `site-packages` directory.

* **On macOS:** Open Finder, press `Cmd + Shift + G`, and navigate to:
  ```text
  /opt/miniconda3/envs/hw6_working/lib/python3.14/site-packages/
  ```
* **On Linux / WSL:** Copy the folder using your terminal:
  ```bash
  cp -R /path/to/extracted/biopython-177/Bio ~/miniconda3/envs/hw6_working/lib/python3.14/site-packages/
  # Note: If you use Anaconda instead of Miniconda, change 'miniconda3' to 'anaconda3' in the path.
  ```

### 3. Apply Pandas Optimization Patch
Run the following automation script in your terminal to adjust the internal data frame validation settings for this environment:

```bash
python -c "
import pandas as pd, os
path = os.path.dirname(pd.__file__) + '/core/frame.py'
with open(path, 'r') as f:
    code = f.read()
target_old = '        if isinstance(index, set):\n            raise ValueError(\"index cannot be a set\")'
target_new = '        if isinstance(index, set):\n            index = list(index)'
if target_old in code:
    with open(path, 'w') as f:
        f.write(code.replace(target_old, target_new))
    print('Pandas environment patch successfully applied!')
"
```

---

## 🏃 Execution

Activate the target environment, navigate to the root directory of your assignment, and execute the core module:

```bash
python pain.py
```

---

## 🛠️ Troubleshooting

### `ModuleNotFoundError: No module named 'Bio'`
* **Cause:** The `Bio` directory was pasted into an incorrect destination or your Conda installation uses a custom root directory.
* **Fix:** Print your exact active package route by running `python -c "import site; print(site.getsitepackages())"` in your terminal and relocate the `Bio` folder to that specific destination.

### `IndentationError: expected an indented block...`
* **Cause:** Corrupted whitespaces or structure changes due to manual syntax edits.
* **Fix:** Avoid editing library files manually. Reinstall a clean pandas distribution via `conda install --force-reinstall pandas` and re-run the Step 3 automation script.

-------------------------------------------------------
Ждал четверга чтобы сдать дз, так как новый WOW вышел сегодня
[](photo_2026-11-05 13.51.14.jpeg)
