# AI Agents Intensive Course (with Google)

This repository contains materials and a short example notebook for the AI Agents Intensive Course using Google ADK and GenAI tooling.

## What’s in this repo

- `5-day-AgenticAI.ipynb` — example Jupyter notebook used during the course.
- `.env` — local environment variables (should not be committed; see below).

## Quick setup (PowerShell)

Pick one approach: Conda (recommended if you use Anaconda/Miniconda) or venv.

### Using Conda (recommended)

```powershell
# create and activate an environment (example name: agenticai)
conda create -n agenticai python=3.11 -y
conda activate agenticai

# upgrade pip and install project deps (example)
python -m pip install --upgrade pip
python -m pip install google-adk

# (optional) install ipykernel so you can use this env in notebooks
python -m pip install ipykernel
python -m ipykernel install --user --name agenticai --display-name "Python 3.11 (agenticai)"
```

### Using venv (lightweight)

```powershell
# create virtual environment inside project
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install google-adk
# register kernel if needed
python -m pip install ipykernel
python -m ipykernel install --user --name agenticai-venv --display-name "Python (agenticai-venv)"
```

### Use the py launcher to pick a specific Python without changing PATH

```powershell
py -3.11 -m pip install google-adk
py -3.11 -c "import sys; print(sys.executable)"
```

## Make sure your terminal uses the right Python

To check which `python` the terminal uses:

```powershell
where.exe python
python --version
python -c "import sys; print(sys.executable)"
py -0p
```

If the wrong version appears, activate the proper conda env or venv before running commands. In VS Code, use the Command Palette → "Python: Select Interpreter" and/or pick the correct kernel in the notebook UI.

## Prevent `.env` from being pushed

1. Add `.env` to `.gitignore` (if not already present):

```powershell
# append .env to .gitignore (creates file if missing)
Add-Content -Path .gitignore -Value ".env" -Encoding UTF8
```

2. If `.env` is already tracked, stop tracking it (keeps the local copy):

```powershell
git rm --cached .env
git add .gitignore
git commit -m "Stop tracking .env and add to .gitignore"
git push
```

### If `.env` was already pushed and contains secrets

- Immediately rotate any secrets (API keys, tokens, passwords) referenced in the file.
- To purge the file from repository history you can use `git filter-repo` (recommended) or BFG Repo-Cleaner. Rewriting history requires coordination with collaborators and a force-push.

Example (high level):

1. Clone a mirror repo, run filter-repo to remove `.env`, then force-push the cleaned repo.

```
# high-level (do not run without reading docs and backups)
git clone --mirror https://github.com/youruser/yourrepo.git
cd yourrepo.git
git filter-repo --path .env --invert-paths
git push --force
```

After history rewrite, all collaborators must re-clone or reset their local repos.

## Dependency management

- Prefer pinning working versions in `requirements.txt`:

```powershell
python -m pip freeze > requirements.txt
```

- If you encounter dependency conflicts (for example: `aext-project-filebrowser-server` requires `watchdog<5` but `watchdog==6.x` is installed), either:
  - Downgrade the conflicting package in that environment:

```powershell
python -m pip install "watchdog>=4.0.1,<5"
python -m pip check
```

  - Or create an isolated environment per project to avoid conflicts.

## Notebook tips

- After creating a venv/conda env, register it as a kernel with `ipykernel` and select it from the notebook kernel menu.
- If you get import errors in the notebook, make sure the kernel interpreter matches the environment where you installed packages.

## Troubleshooting

- "pip: Access is denied" — try `python -m pip` instead of calling `pip` directly. Ensure your PATH and permissions are correct and you are not hitting a locked `pip.exe` from another installation.
- Use `python -m pip check` to list dependency problems.

## Contributing

If you want to improve these notes, open a PR with small, focused edits.

## License

This repo has no explicit license. Add one if you intend to share the materials.
