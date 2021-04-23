# Operating system
macOS Big Sur - Version 11.2.3

# Software suggestion - Development
## Command Line - Homebrew
The Missing Package Manager for macOS. Install from [Official Guide](https://brew.sh) or just run the following.

``` shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Command Line - iTerm2 customization

Terminal emulator for macOS. Download from [Homepage](https://iterm2.com/) or just run the following.

``` shell
brew install iterm2
```

Install `Sauce Code Pro Nerd Font` from [Official Website](https://www.nerdfonts.com/font-downloads).

Oh My Zsh is an open source, community-driven framework for managing your zsh configuration. Install [oh-my-zsh](https://ohmyz.sh/) and [theme](https://iterm2colorschemes.com/) by the following command.

``` shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
brew install romkatv/powerlevel10k/powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Change oh-my-zsh theme in `~/.zshrc` file ([reference](https://github.com/romkatv/powerlevel10k#batteries-included)), and run `exec $SHELL`to take effect:

``` shell
ZSH_DISABLE_COMPFIX="true"

ZSH_THEME="powerlevel10k/powerlevel10k"
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(context dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status time)
DEFAULT_USER="kover"
```

## IDE - Virtual Studio Code

Suitable for being an IDE & Markdown editor. [Installation](https://code.visualstudio.com/)

Plugin recommend:
* Remote-SSH - for developing remotely on another server
* GitLens - for showing Git changelog by the side of codes

## IDE - IntelliJ IDEA
Powerful IDE for Java programming. [Installation](https://www.jetbrains.com/idea/)

Note: Community version is free, but Ultimate version is not.

## Text editor - Sublime Text 3
A sophisticated text editor for coding. [Installation](https://www.sublimetext.com/)

## Container - Docker
A light package software for development, shipment and development. [Installation](https://www.docker.com/)

## Source control - GitHub Desktop
GitHub official desktop platform for source control. [Installation](https://desktop.github.com/)

## Source control - Sourcetree
A free Git GUI client. [Installation](https://www.sourcetreeapp.com/)

# Software suggestion - Productivity
## Alfred 4
Powerful replacement of 'Spotlight Search' with workflow features. [Installation](https://www.alfredapp.com/)

## Google Chrome
Most popular web browser. [Installation](https://www.google.com/intl/zh-CN/chrome/)

## Parallels Desktop
Powerful and user-friendly virtual machine software for running Windows on Mac. Has amazing Coherense mode. [Installation](https://paper.meiyuan.in/)

# Software suggestion - Helper
## Duet
Screen casting software for Mac & Windows and iOS & Android. [Installation](https://zh.duetdisplay.com/)

## Lunar
Intelligent adaptive brightness for external displays. [Installation](https://lunar.fyi/)

## Mac's Fan Control
Mac's fan is set to be as quiet as possible. So if cares about the health of over-heated MacBook, this software can perform real-time monitoring and smart fan control. [Installation](https://www.crystalidea.com/macs-fan-control)

## Paragon NTFS driver
Official driver for Seagate external hard drive to be used on MacOS. [Installation](https://www.seagate.com/cn/zh/support/downloads/item/ntfs-driver-for-mac-os-master-dl/)

## Pap.er
High quality wallpaper setter. [Installation](https://paper.meiyuan.in/)

## The Unarchiver
Powerful app to open RAR on Mac. [Installation](https://theunarchiver.com/)

# Software suggestion - Entertainment
## IINA
Powerful Media player. [Installation](https://iina.io/)

