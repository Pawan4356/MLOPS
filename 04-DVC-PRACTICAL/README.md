
# Data Versioning with DVC and Git

This guide walks through setting up a Git repository with DVC to version your data.

## 🚀 Steps to Follow

### 1. Create a Git repository and clone it locally

```bash
git init my-project
cd my-project
```

### 2. Create `main.py` and add code to save a CSV file

```python
# main.py
import os
import pandas as pd

os.makedirs("data", exist_ok=True)
df = pd.DataFrame({"name": ["Alice"], "age": [25]})
df.to_csv("data/data.csv", index=False)
```

Run the script:

```bash
python main.py
```

### 3. Stage and push the initial code to Git (before using DVC)

```bash
git add .
git commit -m "Initial commit with main.py and data"
git push origin main
```

## ⚙️ Setting Up DVC

### 4. Install DVC

```bash
pip install dvc
```

### 5. Initialize DVC

```bash
dvc init
```

This will create `.dvc/` and `.dvcignore`.

### 6. Create a directory to mimic S3 (or configure your actual S3 remote)

```bash
mkdir S3
```

### 7. Add remote storage

```bash
dvc remote add -d myremote S3
```

## 📂 Tracking Data with DVC

### 8. Add `data/` to DVC

```bash
dvc add data/
```

DVC will warn you that `data/` is already tracked by Git.

Run the following commands:

```bash
git rm -r --cached data/
git commit -m "Stop tracking data folder in Git"
```

### 9. Re-add `data/` for DVC

```bash
dvc add data/
git add data.dvc .gitignore
```

### 10. Commit to DVC and Push

```bash
dvc commit
dvc push
```

### 11. Git commit this state as version 1 of data

```bash
git add .
git commit -m "Version 1 of data with DVC"
git push origin main
```

## 🔁 Modifying and Tracking New Data Versions

### 12. Make changes in your code (`main.py`) to append data

```python
# mycode.py (sample)
import pandas as pd

df = pd.read_csv("data/data.csv")
df.loc[len(df.index)] = ["Bob", 30]
df.to_csv("data/data.csv", index=False)
```

Run:

```bash
python mycode.py
```

### 13. Check DVC status

```bash
dvc status
```

### 14. Commit and push to DVC (Version 2)

```bash
dvc commit
dvc push
git add .
git commit -m "Version 2 of data"
git push origin main
```

### 15. Check DVC/Git status (should be up to date)

```bash
dvc status
git status
```

## 🔁 Repeat for Version 3

Repeat steps 12–14 to add a third version of your data.
