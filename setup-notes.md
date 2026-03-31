# Setup Notes

## Installations

### Mac

**Python**
```bash
brew install python@3.13
```

**Boto3**
```bash
/opt/homebrew/bin/python3.13 -m pip install boto3 --user --break-system-packages
```

**ipykernel**
```bash
/opt/homebrew/bin/python3.13 -m pip install ipykernel -U --user --force-reinstall --break-system-packages
```

---

### Windows

**Python**
> Download and install from: https://www.python.org/downloads/
> Make sure to check **"Add Python to PATH"** during installation.

**Boto3**
```bash
pip install boto3
```

**ipykernel**
```bash
pip install ipykernel
```

---

### Downloads (Mac & Windows)

- **AWS CLI:** https://aws.amazon.com/cli/
- **Visual Studio Code:** https://code.visualstudio.com/
- **Git:** https://git-scm.com/downloads

## VS Code Extensions

### Jupyter
- Jupyter
- Jupyter Keymap
- Jupyter Slide Show
- Jupyter Cell Tags
- Jupyter Notebook Renderers

### Python
- Python
- Pylance
- isort

## GitHub & AWS Setup

- Sign Up for a Free GitHub Account
- Create IAM User With Programmatic Access
- Run the AWS Configure Command
- Create Key Pairs
- Add the Public SSH Key to GitHub
- Create Repo and Clone

## References

- [Boto3 Quickstart Guide](https://docs.aws.amazon.com/boto3/latest/guide/quickstart.html)
