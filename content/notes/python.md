---
title: "Python"
tags:
- software-engineering
- python
disableToc: false
---

## Useful Links
- [Pypi: Find, install and publish Python packages](https://pypi.org/)

## Python Virtual Environment Setup

Using **venv**, you can easily work with multiple projects with various dependencies on the same machine at the same time.

To setup virtual environment or venv on Python, first you will need **PIP**. It is the widely used packet manager for Python.

**PIP** comes bundled with Python installation. On a Mac, Homebrew makes it easier to install Python along with **pip**. Simply, 🔥fire up your terminal and enter the following command:

> brew install python@3.9

The above command installs Python (latest version at the time of the writing) on your Mac. If you already have Python installed on your machine, you can check the version using the following command on you terminal.

> python -V

You can install the latest version of **pip** using the following. This command will automatically install the latest **pip** version.

python3.9 -m pip install --user --upgrade pip

Now, its time for you install the **venv** or **virtual environment** in Python using the following command.

python3.9 -m pip install --user virtualenv

### Create your virtual environment

To create a virtual environment, head to your project directory and run the following command.

python3.9 -m venv venv

### Activating your virtual environment

Before using your virtual environment on your project, you need to activate it using

source venv/bin/activate

Congratulations 🎉, you’ve successfully installed **venv** and activated it. Install em’ packages now 🛠