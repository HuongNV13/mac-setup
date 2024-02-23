---
sidebar_position: 4
---

# Node.js
Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.
## Installation
### Using Homebrew (Not recommended)
```bash
brew install node
```
### Using Node Version Manager (nvm)
Download and install [nvm](https://github.com/nvm-sh/nvm) by running:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
Download Node and select your version by:
```bash
nvm install node        # install most recent Node stable version
nvm install --lts       # install the most recent Node LTS verion
nvm ls                  # list installed Node version
nvm use node            # use stable as current version
nvm ls-remote           # list all the Node versions you can install
```