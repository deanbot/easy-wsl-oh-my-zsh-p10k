# Easy WSL oh-my-zsh setup w/ Powerlevel10k Walkthrough

Follow along to setup [WSL](https://docs.microsoft.com/en-us/windows/wsl/about) and [Z shell](https://en.wikipedia.org/wiki/Z_shell), an alternative to bash with awesome plugin and theme support. This setup uses the popular [Oh My Zsh](https://ohmyz.sh/) Zsh configuration framework to manage plugins and themes (it also comes bundled with a suite of aliases and helpers) and the impressive, [Powerlevel10k](https://github.com/romkatv/powerlevel10k) Zsh theme.

## Table of Contents

1. [WSL and Ubuntu](#WSL-and-Ubuntu)
    1. [Enable features for WSL and WSL 2](#Enable-features-for-WSL-and-WSL-2)
2. [VS Code](#VS-Code)
3. [Windows Terminal](#Windows-Terminal)
    1. [Install Windows Terminal](#Install-Windows-Terminal)
    2. [Update Default Profile](#Update-Default-Profile)
4. [Zsh and Oh My Zsh](#zsh-and-oh-my-zsh)
    1. [Install and setup Zsh](#Install-and-setup-Zsh)
    2. [Install Oh My Zsh](#Install-Oh-My-Zsh)
5. [Powerlevel10k](#Powerlevel10k)
    1. [Install fonts](#Install-fonts)
    2. [Install theme](#Install-theme)
    3. [Run configuration wizard](#Run-configuration-wizard)
    4. [Enable battery indicator](#Enable-battery-indicator)
6. [Install Zsh plugins](#Install-zsh-plugins)
7. [Next Steps](#Next-steps)
8. [Revert to bash](#Revert-to-bash)

## WSL and Ubuntu

The following section details how to install [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) installing the latest LTS of Ubuntu.

### Enable features for WSL and WSL 2

1. Open Windows PowerShell as Administrator
2. Install WSL via:

    ```PowerShell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

[Updating to WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install-win10#update-to-wsl-2) is more involved. WSL 2 requires Windows **Build 19041**. For the time being, the most straight-forward method of updating to this build is to [join Windows Insider](https://insider.windows.com/insidersigninboth/).

To verify your which Windows build you are running, open **Settings** and select **System**. Scroll to **Windows specifications** and note the value for **OS build**.

If you meet the requirements:

1. Enable the required feature:

    ```PowerShell
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```

2. Restart your machine.
3. Open Windows PowerShell and run the command: `wsl --set-default-version 2`

_I skipped enabling WSL 2, and it hasn't issues as far as I can tell._ I will likely revisit this once the build is distributed via Windows Update.

### Install Ubuntu

To install the latest Ubuntu LTS version (i.e. 20.04):

1. Open Windows Store and search for Ubuntu ([quick link](https://aka.ms/wslstore)). Select **Ubuntu** (note you can verify the Ubuntu version by selecting **More**) and then select **Install**.
2. Launch **Ubuntu** to complete installation.
3. [Create a user account](https://docs.microsoft.com/en-us/windows/wsl/user-support) by entering a **User Name**, i.e. your first name (typically lowercase) and a password.

    See also: [Change or recover your password](https://docs.microsoft.com/en-us/windows/wsl/user-support#reset-your-linux-password)

4. Update packages enter: `sudo apt update && sudo apt upgrade`

    Note you may need to right click or `ctrl+shift+v` to paste.

    Press `[enter]` to select `Y` at the prompt to install any package updates.

We'll switch to Windows Terminal in a following step, as the default WSL/Ubuntu terminal emulator is not the greatest.

## VS Code

If you're a VS Code user some light setup is required for proper WSL use, namely installation of the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension or the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension pack.

**Remote - WSL** contains just the requirements for using WSL where as **Remote Development** contains a bundle of remote dev tools.

VS Code will prompt you to install **Remote - WSL** after it detects that WSL is installed. Alternatively, install either via the **Install** button on the extension page or via the extensions tab in VS Code.

See also: [[VS Code:] Developing in WSL](https://code.visualstudio.com/docs/remote/wsl)

## Windows Terminal

This section details how to install and configure [Windows Terminal](https://aka.ms/terminal).

### Install Windows Terminal

To install Windows Terminal, open **Windows Store**. Search for and install **Windows Terminal**. I suggest pinning Windows Terminal to your start menu or taskbar.

### Update Default Profile

The default profile shown when you launch Windows Terminal is Windows PowerShell. Selecting the dropdown reveals Ubuntu.

To change the default profile:

1. Enter `ctrl+,` to open the settings file for Windows Terminal in you default editor.
2. Locate the `guid` for WSL/Ubuntu and then copy and paste this value in the `defaultProfile` property.

### Configure Windows Terminal

See [settings.json](./settings.json) in this repo for reference.

1. Include the following settings for `profiles.defaults`:

    ```json
    "defaults": {
        // Put settings here that you want to apply to all profiles.
        // "fontFace": "MesloLGS NF",
        "fontSize": 9,
        "snapOnInput": true,
        "historySize": 9001,
        "acrylicOpacity": 0.85,
        "useAcrylic": true,
        "closeOnExit": true
    },
    ```

    Leave fontFace commented until you [install the font](#Install-fonts) in a following section.
2. Hide irrelevant profiles by setting the `hidden` property to false.

## Zsh and Oh My Zsh

The following section details how to install Zsh and [Oh My Zsh](https://ohmyz.sh/).

### Install and setup Zsh

To install Z shell and set it as your default shell run:

```sh
sudo apt install zsh
chsh -s $(which zsh)
```

Log out and login back again to use your new default shell.

If you have no `.zsh[...]` files in your home directory you will be prompted. Feel free to run through the configuration wizard however this will get replaced by Oh My Zsh. I recommend selecting `2` at the prompt to create a `.zshrc` with defaults.

### Install Oh My Zsh

To install Oh My Zsh run:

`sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

If you had a `.zshrc` file it's now backed up as `.zshrc.pre-oh-my-zsh` and replaced with a new configuration file which loads Oh My Zsh.

## Powerlevel10k

The following section outlines how to install the [Powerlevel10k](https://github.com/romkatv/powerlevel10k) Zsh theme. The recommended font for pl10k works well in Windows Terminal.

### Install fonts

First, manually install the recommended Meslo Nerd Font (patched for pl10k) - [found here](https://github.com/romkatv/powerlevel10k#manual-font-installation).

To install, download and double-click each variation and select install.

_Note: If using Windows Terminal, enable the font by adding `"fontFace": "MesloLGS NF"` to `profile.defaults` or the WSL profile._

### Install theme

To install pl10k:

1. Run: `git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k`
2. Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`.

### Run configuration wizard

Restart your shell by logging out logging back in. The configuration wizard should start automatically, otherwise run `p10k configure`.

Follow prompts to configure pl10k to your liking. First, it prompt you to verify whether you see given icons. Afterwards, you'll be prompted for ui preferences. **These are up to you.** The following choices work well but go crazy.

* Prompt Style: Rainbow
* Character Set: Unicode
* Show Current Time: 24-hour format
* Prompt Separators: Slanted
* Prompt Heads: Sharp
* Prompt Tails: Flat
* Prompt Height: Two Lines
* Prompt Connection: Solid
* Prompt Frame: Left
* Connection & Frame Color: Dark
* Prompt Spacing: Sparse
* Icons: Many Icons
* Prompt Flow: Consise
* Enable Transient Prompts: yes
* Instant Prompt Mode: Verbose

If you don't like what you came up with restart or remove your `.p10k.zsh` file.

See [p10k.zsh](./p10k) in this repo for reference.

The configuration I'm using at any given time is found in my [deanbot/dotfiles](https://github.com/deanbot/dotfiles) repo.

### Enable battery indicator

1. Edit `.p10k.zsh`
2. Uncomment the `battery               # internal batter` line in the list of `POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS` and save.
3. Run: `source ~/.p10k.zsh` to see changes.

## Install Zsh Plugins

Most plugins are installed automatically with Oh My Zsh by editting `plugins` in `.zshrc`.

The following suggested plugins have minimal installation steps:

* [zsh-autosuggestion](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)
* [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md#oh-my-zsh)

To install [zsh-autosuggestion](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md) and [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md#oh-my-zsh):

1. Run the following commands one at a time:

    ```sh
    git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
    ```

2. Permissions on these two plugins may be too permissive (you'll be warned by zsh the next time you log into your shell). To correct this run:

    ```sh
    cd $ZSH_CUSTOM/plugins/
    sudo chmod 755 ./zsh*
    cd ~
    ```

3. Edit `.zshrc` via your preferred editor be it nano, [micro](https://micro-editor.github.io/), VSCode, etc (i.e. for VSCode `code ~/.zshrc`).
4. update the `plugins` line to: `plugins=(git zsh-autosuggestion zsh-syntax-highlighting)` and save.
## Next Steps

Where to go from here? Some suggestions in no particular order:

* Review/adjust Windows Terminal [keybindings](https://docs.microsoft.com/en-us/windows/terminal/customize-settings/key-bindings).
* Extend dot files with your desired aliases or path modifications.
* Familiarize yourself with [Zsh](http://zsh.sourceforge.net/) and browse [tips & tricks](https://github.com/hmml/awesome-zsh) from the community.
* View the Oh My Zsh [cheatsheet](https://github.com/ohmyzsh/ohmyzsh/wiki/Cheatsheet).
* Explore the Oh My Zsh [plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) directory.
* [Customize](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) pl10k.

## Revert to Bash

If you'd like to return from Zsh to bash this can be easily done:

1. **Uninstall Oh My Zsh** ([ref](https://github.com/ohmyzsh/ohmyzsh#uninstalling-oh-my-zsh)): `uninstall_oh_my_zsh`
1. **Set default shell to bash**: `chsh -s $(which bash)`
