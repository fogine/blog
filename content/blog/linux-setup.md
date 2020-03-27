+++

title = "My working environment"
slug = "my-working-environment"
date = 2020-03-14
[taxonomies]
tags = ["unixporn", "linux", "archlinux", "i3wm", "tmux", "zsh", "vim", "vifm", "dmenu", "rofi"]
[extra]
render_toc = true
+++

As I spend significant amount of time working with computers, it's important to me to have a desktop environment which does not distract me from work.  

Moreover I expect my system environment to help me be more productive and to complement my habits in context of how I interact with the environment.  

In this post I'm gonna describe how is my linux installation setup and main tools I use on every day basic for past 6 years and why.  
Do not expect configuration tutorial but rather high-level toolset overview.

<!-- more --> 

## OS distribution - Archlinux

![arch-vs-ubuntu](/images/arch_vs_ubuntu.png)

As majority of current `Arch` users, I started using `Xubuntu` (Ubuntu with `xfce` desktop environment instead of `gnome`) as my first `UNIX` based OS.  

`Xubuntu` was certainly better than `Windows` as it allowed me to "hack" it around and more importantly, it had a proper `shell`.  

However, at the end of the day, it was my unorthodox doing that led to all kind of crashes during system upgrade and ultimately was forcing me to reinstall the system.  

Lesson learned: `Ubuntu` is a robust preconfigured system made for general public, not necessary for developers, serving as out of the box conventional desktop everybody is familiar with.  

It wasn't until some years later when a colleague of mine introduced me to `Archlinux` distro, that solved most of my issues.

These are [Archlinux](https://www.archlinux.org/) features that make it a superior distribution for me personally:  

- It includes ***base system only*** and its up to you to install and configure everything else that you need.
- Arch ***uses rolling release system*** as opposed to Ubuntu's discrete release system - this means that packages are always up to date, you don't have to wait 6 months for system upgrade nor use custom repositories with updated package versions. This is a big one for me! 
- Its [***WIKI pages***](https://www.archlinux.org/) are far from any other knowledge base available. They provide comprehensive and well-arranged information about `Linux` in general. Its my GO-TO information source for `Linux`.

## System configuration - dotfiles

All configuration files are tracked in (public) `git` [repository](https://github.com/fogine/dotfiles) and reside in single `.dotfiles` directory (check it out if you want to see how something is configured).  

In there, configuration files are organized in separate folders by topic and symlinked into `$HOME` when you run `script/bootstrap`.  

This makes it really simple to add and edit configuration and when you need to reinstall once in roughly 5 years, you run single command to get everything setup exactly how it was before.

## Window manager - i3wm

[i3](https://i3wm.org/) is a tiling window manager.  
Tiling window managers organize windows in a grid of tiles so that 100% of the screen space is always in use.

![i3wm tiled windows](/images/i3wm-tiles.png)

**Note:** You can still have classic floating windows but those are almost always redundant.  

Usually, the user creates, deletes, moves, resizes or focuses windows with shortcuts (in addition to mouse navigation). Mine are defined in such way so that they match vim-like window manipulation mappings:

| Action              | Shortcut |
|---------------------|----------|
| Focus left window   | Super+h  |
| Focus right window  | Super+l  |
| Focus up window     | Super+k  |
| Focus bottom window | Super+j  |
| Close window        | Super+q  |
| ...                 | ...      |

Since I use `Vim` as my main editor, having the same shortcuts that do the same thing globally at system level, not just in the editor, makes it really convenient.  
I'm too lazy to travel with my right hand from keyboard to mouse and back just so I can focus different window...  

There are also 10 workspaces available which can contain different running applications. I switch between them with `Super+<0-9>` mappings.

## Terminal multiplexer - tmux

> [tmux](https://github.com/tmux/tmux) is a terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal.  
>
> *source: [https://github.com/tmux/tmux/wiki](https://github.com/tmux/tmux/wiki)*


## Shell - zsh

What makes [zsh](http://zsh.sourceforge.net/) superior to regular `bash` are its `autosuggestions` & `tab-completion` & `syntax-highlighting` features.  

Lets look at some examples:

{{ figure(src="/images/zsh-tab-completion.gif" alt="zsh tab completion" caption="zsh tab completion") }}

The grey text behind cursor that you can see in the preview suggests best matching command based on what you have typed so far and on your shell command history.  

You can accept suggested command anytime with `Ctrl+e`.  

Moreover if you want to see list of available commands or arguments from man pages, you just press `Tab` key.  

Also you can notice that `git` command is highlighted with green-ish color - that signalizes that command is recognized and the shell can find appropriate file with executable flag.  

I also use some navigation tools that make a shell whole a lot more intuitive:  

- ### z: jump around  

    `z` tracks your most used directories, based on 'frecency'.
    
    After  a  short  learning  phase, `z` will take you to the most 'recent'  
    directory that matches a pattern given on the command line.
    
    ![zsh directory navigation](/images/zsh-directory-navigation.gif)
    
    In addition to `z`, I've configured number aliases `1`,`2`...and so on to `cd -1`, `cd -2`... which change your current dirrectory to the previous one.

- ### Ctrl-R: search command history
    This [utility](https://github.com/zsh-users/zsh-history-substring-search) plays really well with the other `zsh` features as it complements completion of more complex commands that I rarely use and can't quickly type out off the top of my head.
    {{ figure(src="/images/zsh-search-history.gif" alt="zsh search history" caption="zsh search history") }}

## Editor - vim

`vim` does not need an introduction. Everybody in the field has at least heard of it. And if I exaggerate a bit, its either the greatest piece of software or source of fear and frustration.  

It's out of the scope of this article to go through individual mappings and plugins I use.  
However if you want to see the plugin list, its available [HERE](https://github.com/fogine/dotfiles/blob/master/vim/vim.symlink/vimrc#L12).  

{{ figure(src="/images/urxvt-tmux-vim.png" alt="vim editor" caption="vim editor") }}

What are those fancy arrow-like UI segments? You are asking... Those are custom status lines for `vim` & `tmux` and `zsh`, powered by [powerline](https://powerline.readthedocs.io/en/master/overview.html) plugin.

## File manager - vifm

[vifm](https://github.com/vifm/vifm/) is a curses based vim-like file manager.  
It's like `Total Commander` but better.  
If you use `vim`, you will love `vifm`.  

{{ figure(src="/images/vifm.png" alt="vifm" caption="vifm") }}

## System-wide interface - dmenu / rofi

If you got all the way down here, you may be asking where are some buttons, where is my system bar with time and tray area with application icons or start menu? The answer is simple, there are not any.  

I have found out I do not need any system wide status bar with icons and basic information like date-time etc... It would be wasting of screen space the most of the time anyway and source of distraction.  

So how do I start applications, turn off the computer, connect to wifi or manage removable devices like usb flash drives, you are asking? I use `dmenu` or `rofi`.  

### What is dmenu and rofi
`dmenu` is a dynamic menu. The user gives it possible menu options as the input, and it creates a graphical menu with one item per line. The user can then select an item, through the arrow keys or typing a part of the name, and selected option is returned as the output. The user then takes the output, lets say it's `Power-off` and implements the action - powers down the computer.  

`rofi` is more powerful `dmenu`  

So in practice, applications are built around `dmenu` that are focusing on single task, like system power management, wifi connection, mouting/umouting removable devices etc...  

You bind the application to a shortcut (or button) eg. `Super+m` and dmenu will popup a list eg.: of connected flash drives that you can mount or umount.

{{ figure(src="/images/rofi-app-launcher.png" alt="rofi app launcher" caption="Example: application launcher") }}

{{ figure(src="/images/udiskie-dmenu.gif" alt="udiskie dmenu" caption="Example: mouting/umouting removable devices (eg.: flash drives)") }}
