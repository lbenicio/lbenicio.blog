---
type: "posts"
title: Terminal like a pro
icon: fa-comment-alt
tags: terminal, customization, theming, tutorial
categories: ["tutorial"]

date: "2022-06-19"
---


It is my first post, or, in programming language, it means, a hello world post. So, let's talk about on how to tweak your terminal to make it look like the picture above. Let's dive in. To summarize, we need to tie together a couple of solutions, Nerd Fonts, Oh-My-ZSH, bash history, auto-completions, syntax highlight and interactive-cd plugins, powerlevel9k theme, and iTerm2 (for macOS users).

![Tutorial's final terminal view](/assets/imgs/terminalView.png){:class="img-fluid"}

## Install Iterm2

**This section is only intended for macOS users, so if you are using any Linux distro, you can skip ahead to the next section safely.**

![iTerm2 logo](/assets/imgs/iterm2Logo.jpeg){:class="img-fluid"}

This is the easiest part of the tutorial. Just go ahead on Term2 Downloads Page and grab the latest version.

[https://iterm2.com/downloads.html](https://iterm2.com/downloads.html)

After that, it is just like any other macOS software, drag it to the applications' directory.

## Install Oh-my-zsh

![Oh-my-zsh logo](/assets/imgs/ohMyZshLogo.png){:class="img-fluid"}

Next step will be to install oh-my-zsh, a plugin for the zsh shell. If you are using another shell language on your terminal, you will have to switch to zsh to use Oh-my-zsh.

Here it is the download link for Oh-my-zsh: [https://ohmyz.sh/#install](https://ohmyz.sh/#install)

Or, for short, just execute the command below:

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

```bash
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

## Install PowerLevel9k theme

![PowerLevel9k logo](/assets/imgs/powerlevel9kLogo.png){:class="img-fluid"}

Now we are starting to personalize our terminal view. This is step is about installing the theming engine, which will allow us to display all those pretty info you saw above.

Here is the GitHub link for Powerlevel9k: [https://github.com/Powerlevel9k/powerlevel9k](https://github.com/Powerlevel9k/powerlevel9k)

Or, if you prefer, a direct link to the installation instructions to all sorts of Operating Systems:

[https://github.com/Powerlevel9k/powerlevel9k/wiki/Install-Instructions#step-1-install-powerlevel9k](https://github.com/Powerlevel9k/powerlevel9k/wiki/Install-Instructions#step-1-install-powerlevel9k)

## Install Nerd Fonts

![Nerd Fonts logo](/assets/imgs/nerdFontsLogo.svg){:class="img-fluid"}

The next step of our configuration is the Fonts, after all, how could you end up using all those emojis on our terminal? Nerd Font is a fonts pack that we are going to use for that. And, actually, it is required to Powerlevel9k work properly.

Just go ahead on their GitHub and follow the installation instructions:

[https://github.com/ryanoasis/nerd-fonts](https://github.com/ryanoasis/nerd-fonts)

Or, if you prefer, direct to install instructions: [https://github.com/ryanoasis/nerd-fonts#font-installation](https://github.com/ryanoasis/nerd-fonts#font-installation)

Here I would advise installing the hole package instead of selected fonts, for easy of install and maintenance. But you want to save some hard drive space, you can install only the one we need, which is 'nerdfont-complete'.


## Install plugins

We are almost there, we just need to install a couple of plugins for easy of day to day life. Interesting features. Let's go.

### zsh-syntax-highlighting

Our first plugin is ZSH Syntax Highlighting, and, as the name already says, it is a syntax highlight plugin. This will help us better visualize our terminal shell while using it in interactive mode.

Here it is the link to their installation page:

[https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)

### zsh-autosuggestions

ZSH Auto Suggestion is a plugin to suggest a command as you type, based on your history and completions, as stated on their GitHub page.

Here it is the installation instructions: [https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

### zsh-interactive-cd

The interactive ```cd``` will show you directories to navigate to while you type the ```cd``` command on your shell. 

Here is the GitHub: [https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/zsh-interactive-cd](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/zsh-interactive-cd)

Installation:

Create the directory to store the plugin files

```bash
mkdir ~/.oh-my-zsh/custom/plugins/zsh-interactive-cd
```

Move the plugin file to the directory

```bash
mv zsh-interactive-cd.plugin.zsh ~/.oh-my-zsh/custom/plugins/zsh-interactive-cd/zsh-interactive-cd.plugin.zsh
```

Import the file on your zshrc

```bash
echo "~/.oh-my-zsh/custom/plugins/zsh-interactive-cd/zsh-interactive-cd.plugin.zsh" >> ~/.zshrc
```

### zsh-completions

As stated on their GitHub page, ZSH Completions "aims at gathering/developing new completion scripts that are not available in Zsh yet". SO, let us jump in.


Here is the link to their repository: [https://github.com/zsh-users/zsh-completions](https://github.com/zsh-users/zsh-completions)

## Tweak zshrc file

Finally, we are almost there, if you made through all this installation, now we just need to instruct our oh-my-zsh enabled Terminal to personalize it the way we want. Let's cut further explanations and go direct to the settings:

First, we need to add our theme:

```shell
ZSH_THEME="powerlevel9k"
```

Set the powerlvel9k theme mode, like, it will use all those emojis:

```shell
POWERLEVEL9K_MODE="nerdfont-complete"
```

Add the prompt we use to show all those pretty information you saw on our first screenshot:

```shell
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(dir rbenv vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status root_indicator background_jobs history time)
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
```

Add a space in the first prompt:

```shell
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX="%f"
```

Visual customization of the second prompt line

```shell
local user_symbol="$"
if [[ $(print -P "%#") =~ "#" ]]; then
    user_symbol = "#"
fi
```

The second line of our prompt:

{% raw >}}
```shell
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="%{%B%F{black}%K{yellow}>}} $user_symbol%{%b%f%k%F{yellow}>}} %{%f>}}"
POWERLEVEL9K_VCS_MODIFIED_BACKGROUND='red'
```
{% endraw >}}

Enable connections from default oh-my-zsh configurations:

```shell
ENABLE_CORRECTION="true"
```

Just make sure that all those plugins are imported here, on the plugins sections, and, as well as, properly sourced.

```shell
plugins=(
zsh-completions
zsh-autosuggestions
zsh-syntax-highlighting
zsh-interactive-cd   
)
```

You may need to manually set your language environment:

```shell
export LANG=en_US.UTF-8
```

Preferred editor for local and remote sessions:

```shell
if [[ -n $SSH_CONNECTION ]]; then
   export EDITOR='nano'
else
   export EDITOR='nano'
fi
```

Yeah, I am a nano guy myself.


```shell
function title()
{
   # change the title of the current window or tab
   echo -ne "\033]0;$*\007"
}
```

Revert the window title after the ssh command:

```shell
function ssh()
{
   title $1
   /usr/bin/ssh "$@"
    #   title "🏡 " $USER@$HOST
   title $USER@$HOST
}
```

Revert the window title after the su command:

```shell
function su()
{
   /bin/su "$@"
   title $USER@$HOST
}
```

Some custom colors:

```shell
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad
```

Extra, a alias:


```shell
alias ls='ls -GFh
```

Also, I would suggest this amazing blog post: [https://www.cyberciti.biz/tips/bash-aliases-mac-centos-linux-unix.html](https://www.cyberciti.biz/tips/bash-aliases-mac-centos-linux-unix.html)


## Tweak iTerm2 

This section, again, is only for macOS users, here we are going to tweak some settings on iTerm2:

The first tweak is to set the iTerm2 to restore previous sessions from where you left when you close the Terminal. This is pretty handy, and if you want to go back to your default directory, just type a ```cd``` command.

To do so, go to Preferences → Profile and edit as the screenshot below:

![Screenshot showing settings of Working directory set to Restore previous session](/assets/imgs/iterm2RestorePreviousSessssion.png){:class="img-fluid"}

Now we move to the 'text' tab, still inside the Profile tab of the preferences window. Here, we need to set our Nerd Font as our default font for the terminal. 

![Screenshot showing settings of Font selection set to Hack Nerd Font Mono](/assets/imgs/iterm2HackNerdFont.png){:class="img-fluid"}

As shown on the screenshot above, on Font section, select 'Hack Nerd Font Mono'. Here I would suggest use the mono spaced font for better visualization of code. Seriously, does anyone code without mono spaced fonts?

Now this one tweak is kind of fun with the old good days. Move to the window tab, still on profile tab. Here we need to tweak the default window size for our terminal, and follow the first TTY, let set it to 80 columns wide.

![Screenshot showing settings for new windows set to columns 80](/assets/imgs/iterm2ttyColumns.png){:class="img-fluid"}

Last but not least (sorry, I had to do it), we need to tweak our colors. I, personally, find it too matte, let us add some bright.

Move back to the color tab, still on Profile tab of the Settings panel.

![Screenshot showing settings for colors](/assets/imgs/iterm2ColorScheme.png){:class="img-fluid"}

Here, modify the HTML codes for the as follows:

|         | Normal | Bright |
|