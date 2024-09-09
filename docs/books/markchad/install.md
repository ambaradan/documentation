---
title: Installation
author: Franco Colussi
contributors: Steven Spencer
tested_with: 9.4
tags:
  - neovim
  - nvchad
  - editor
  - markdown
---

## Introduction

The Markchad configuration is distributed complete with all the files needed to run it, once installed it can be used immediately, the configuration files found in `~/.config/nvim` and the shared files in `~/.local/share/nvim` are provided.  
This solution provides a more stable package than a distribution with *Git*, so plugins, which by their nature are constantly being updated, can be carefully configured and tested before being included in the configuration to be distributed.  
The solution also makes it possible to distribute along with the configuration files the language servers (LSPs), linters and formatters necessary for the plugins to function.

!!! note "Standard NvChad installation"

    The traditional NvChad installation procedure of first installing plugins and then, with manual intervention, installing the necessary LSPs does not allow the deployment of plugins that require the language servers already installed to build them.

## :material-cart-plus: Requirements

The configuration for its installation requires the same components required by NvChad, which are:

* *Neovim* 0.10
* A *Nerd Font* installed and configured in the terminal
* *git* installed in the system
* *GCC* and *Make*

For a more in-depth overview of the necessary components, consult the [NvChad installation](https://nvchad.com/docs/quickstart/install) page.

### :material-tray-plus: Additional requirements

The following components are also required for successful configuration:

* [sqlite](https://sqlite.org/) (lightweight database) used by *yanky.nvim* to store copied strings
* [ripgrep](https://github.com/BurntSushi/ripgrep) (search tool) a line-oriented search tool
* [lazygit](https://github.com/jesseduffield/lazygit) an ncurses-style interface that allows you to perform all git operations in a more user-friendly way

#### Installation of additional packages

Installation of add-on packages in Rocky Linux comes from various sources. These procedures are:

!!! note

    The following commands assume that you are using the *root* account or have `sudo` privileges. If using `sudo`, append `sudo` to the command listed. 

The `sqlite` package is available from the official Rocky Linux repositories so just install it with the package manager:

```bash
dnf sqlite -y
```

For the installation of `ripgrep`, the EPEL repository is needed. A [dedicated section](https://docs.rockylinux.org/books/admin_guide/13-softwares/?h=#the-epel-repository) in the Administrator's Guide is available for an overview of the project.  
It involves executing the following commands:

```bash
dnf install epel-release
dnf update
dnf install  ripgrep -y
```

The third required package, `lazygit`, is not available in the official channels (Rocky Linux, EPEL), but installation from Fedora's [Repository Copr](https://copr.fedorainfracloud.org/) is possible with the commands:

```bash
dnf copr enable atim/lazygit -y
dnf install lazygit
```

## :material-monitor-arrow-down-variant: Installation

Installation consists of copying the repository (clone) to a folder within the user's configuration path `~/.config/`, a path used by *Neovim* to search for the configuration to insert.  
Two scenarios are possible:

1. Using the primary configuration as the main configuration, replacing the default configuration of *Neovim*
2. Installing separately and using it independently of the main configuration

The second solution allows you to test it without compromising your default installation, leaving the default configuration for use of other development languages you may use.


## :material-update: Configuration update

Neovim's plugin ecosystem for markdown code is relatively young and in full ferment consequently it can be useful to update the cloned configuration to reflect the changes. You update in the usual way, using *git*, but when finished it requires an additional step. That is to bring the plugin state to that present on the repository, with the command:

```text
:Lazy restore
```

The command uses the plugin states (*commit*) saved in the `lazy-lock.json` file to bring all plugins to the state present in the configuration, thus ensuring maximum adherence of the local configuration with the remote one.

## :material-contain-end: Conclusions

The configuration is constantly evolving and tries to follow the development of the main plugins for writing markdown code. The author uses it daily, so it should be stable enough for use in *production*. Any kind of suggestions or corrections are welcome.
