---
sidebar_position: 5
---

# Python
macOS, like Linux, ships with [Python](https://python.org/) already installed. But you don't want to mess with the system Python (some system tools rely on it, etc.), so we'll install our own version(s).
## Installation
### Using Homebrew (Not recommended)
```bash
brew install python
```
### Using Simple Python Version Management (pyenv)
[pyenv](https://github.com/pyenv/pyenv) lets you easily switch between multiple versions of Python. It's simple, unobtrusive, and follows the UNIX tradition of single-purpose tools that do one thing well.

Install [pyenv](https://github.com/pyenv/pyenv) by running:
```bash
brew update
brew install pyenv
```
After installing, add pyenv init to your shell to enable shims and autocompletion (use .zshrc if you're using zsh).
```bash
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
```
Download Python and select your version by:
```bash
pyenv install --list    # list all available version to install
pyenv install 3.9.0     # install the desired version
pyenv global 3.9.0      # set global version
```