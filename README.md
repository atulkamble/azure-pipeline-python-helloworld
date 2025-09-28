# azure-pipeline-python-helloworld

Gotcha, Atul! Here’s a minimal, copy-paste Azure Pipelines setup to run a Python `helloworld.py` on Ubuntu.

# 1) Repo layout

```
your-repo/
├─ helloworld.py
└─ azure-pipelines.yml
```

**helloworld.py**

```python
print("Hello, Azure Pipelines from Ubuntu!")
```

# 2) Azure Pipelines YAML (Ubuntu hosted)

Create **azure-pipelines.yml** at repo root:

```yaml
# Run on every push to main; tweak as needed
trigger:
  branches:
    include:
      - main
# Optional PR validation
pr:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'  # Microsoft-hosted Ubuntu

steps:
  - checkout: self

  # Ensure a specific Python version is on PATH
  - task: UsePythonVersion@0
    inputs:
      versionSpec: '3.11'
      addToPath: true

  # (Optional) install deps if you add requirements.txt later
  - script: |
      python --version
      pip --version
      if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
    displayName: Setup Python & deps

  # Run your script
  - script: python helloworld.py
    displayName: Run hello world
```

# 3) Quick start

1. Commit & push:

   ```bash
   git add helloworld.py azure-pipelines.yml
   git commit -m "Add Azure Pipeline for Python hello world"
   git push origin main
   ```
2. In **Azure DevOps**: Pipelines → **New pipeline** → pick your repo → “Existing Azure Pipelines YAML file” → select `/azure-pipelines.yml` → **Run**.

# 4) Notes & common tweaks

* Change Python version: update `versionSpec` (e.g., `3.12`).
* Add dependencies: create a `requirements.txt` and they’ll be installed automatically by that conditional line.
* If you prefer a **self-hosted** Ubuntu agent, replace `pool` with your agent pool name:

  ```yaml
  pool:
    name: 'Default'   # or your custom pool
    demands:
    - Agent.OS -equals Linux
  ```

That’s it—super lightweight and works out of the box on Microsoft-hosted Ubuntu. Want me to add a pytest job or artifact publish step next?
