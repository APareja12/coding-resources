# Python Virtual Environments

## Overview

Virtual environments are isolated Python environments that allow you to install packages and dependencies specific to a project without affecting your system-wide Python installation. They solve the common problem of dependency conflicts between different projects and make it easy to share exact package versions with other developers. Think of them as separate containers for each project's Python setup.

## Key Concepts

- **Isolation** - Each project has its own independent set of installed packages
- **Reproducibility** - Easy to recreate the exact same environment on another machine
- **No conflicts** - Different projects can use different versions of the same package
- **System protection** - Your global Python installation stays clean and unmodified
- **Requirements file** - `requirements.txt` lists all packages and versions for easy sharing

## Code Example

```bash
# Creating a virtual environment
# Using venv (built into Python 3.3+)
python3 -m venv myenv

# Or on some systems:
python -m venv myenv

# Activating the virtual environment
# On macOS/Linux:
source myenv/bin/activate

# On Windows:
myenv\Scripts\activate

# Your prompt will change to show the active environment:
(myenv) user@computer:~/project$

# Installing packages (only affects this virtual environment)
pip install requests
pip install django==4.2.0
pip install pandas numpy matplotlib

# Viewing installed packages
pip list
pip freeze  # Shows exact versions

# Saving dependencies to a file
pip freeze > requirements.txt

# Installing from requirements.txt (on another machine or new environment)
pip install -r requirements.txt

# Deactivating the virtual environment
deactivate

# Your prompt returns to normal:
user@computer:~/project$
```

```python
# Example requirements.txt file
requests==2.31.0
django==4.2.0
pandas==2.0.3
numpy==1.24.3
pytest==7.4.0
python-dotenv==1.0.0
```

```bash
# Common workflow for a new project
mkdir my-project
cd my-project

# Create virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate  # macOS/Linux
# or
venv\Scripts\activate     # Windows

# Install packages
pip install flask sqlalchemy

# Save dependencies
pip freeze > requirements.txt

# Add venv/ to .gitignore (don't commit the virtual environment)
echo "venv/" >> .gitignore

# Work on your project...
python app.py

# When done, deactivate
deactivate
```

## Common Use Cases

- **Starting new projects** - Create a clean environment for each project
- **Collaborating with teams** - Share requirements.txt so everyone has identical packages
- **Testing different versions** - Test your code against different package versions
- **Deploying applications** - Ensure production has exact same dependencies as development
- **Learning new packages** - Experiment without polluting your system Python

## Additional Resources

- [Python Documentation: venv](https://docs.python.org/3/library/venv.html)
- [Real Python: Virtual Environments Primer](https://realpython.com/python-virtual-environments-a-primer/)
- [Python Packaging Guide](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/)
- [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/) - Advanced virtual environment management
