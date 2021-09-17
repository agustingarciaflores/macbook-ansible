# macbook

The ansible playbook in this repository sets up my MacBook I use to work with [Anaconda](https://anaconda.com) with all tools I need.

The setup this playbook provides is opinionated as its targeted at my usage patterns and workflows with e.g. Neo2 Keyboard layout, VS Code as IDE, keepassxc as password manager.

Check [the „Deployment“ section](#Deployment) for instructions on how to only install the basic toolset.

## Tools and configurations

### Terminal

The playbook sets up [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) with [powerlevel10k](https://github.com/romkatv/powerlevel10k) for my preferred shell experience. I use `iterm2`, therefore the recommended fonts are installed automatically.

If you use another terminal, check [the installation instructions](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k)

### Keyboard layout

I use the ergonomic [Neo2](https://neo-layout.org/) keyboard layout which is optimised for German texts, but also has benefits for English texts and especially for programming: Almost all needed special characters are mapped on the third layer (of six in total) so they’re extremely easy to type.

To remap the different levels, it uses `Karabiner Elements` which is also installed by this playbook.

For the full set up, some manual steps are needed, check the [macOS install instructions for details](https://neo-layout.org/Einrichtung/macOS/).

### Workflows

I use [ghq](https://github.com/x-motemen/ghq) to simplify the organization of git repositories. It is automatically set up by the playbook to use `~/repos` as base directory.

It also sets up [pre-commit](https://pre-commit.com/) which I use to e.g. enforce code and commit style.

The git config assumes that your username is `[first letter of first name][last name]` and sets `[username]@anaconda.com` as git user email.
It also sets your full name as the git user name.

### Miscellaneous

Please check the `homebrew_cask | Install casks` and `homebrew | Install` for various little helpers that are installed with this playbook (e.g. `iperf3`, `mtr`).

## Repository set up

After cloning the repository and the first run of the playbook, set up pre-commit (or install pre-commit manually instead of running the playbook):

```sh
ansible-galaxy install -r requirements.yaml
pre-commit install --hook-type commit-msg --hook-type pre-commit
```

## Deployment

If you want to only install the basic toolset, always add

```sh
--skip-tags opinionated
```

### First run

For a first run on a new MacBook, use:

```sh
# Download homebrew installer. IMPORTANT: Review the script BEFORE executing it!
curl -fsSL -o install-brew.sh https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh

# Install homebrew
/bin/bash install-brew.sh

# Install ansible
brew install ansible

# Run the playbook
ansible-playbook macbook.yaml --ask-become-pass
```

After the initial deployment

1. Restart iterm and run `p10k configure`
2. Answer with „Yes“ once asked if you want to install the Meslo LGS Nerd Font and quit iterm once the installation is complete
3. If opened: Restart Visual Studio Code to fix the terminal font

### Subsequent runs

To run the playbook, use

```sh
ansible-playbook macbook.yaml --ask-become-pass
```

If you only want to update and install packages with homebrew, you can use

```sh
ansible-playbook macbook.yaml --tags packages
```
