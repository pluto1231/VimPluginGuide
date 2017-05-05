# VimPluginGuide
My commonly used Vim plugin with installation instructions

## Intro

Vim with the right plugins can greatly improve the productivity of a programmer. While there are many vim "bundles" with many preconfigured plugins that you can install in bulk, I personally prefer to pick my own plugins one by one so I unsderstand what each of them do, and it's less confusing for people who just are just getting started with "tweaking" vim.

I would start with a "plugin manager" that will make install/uninstalling plugins much easier. There are several options, I personally use [Vundle](https://github.com/VundleVim/Vundle.vim/).

This guide currently assumes the reader is on a Linux environment. If you're using vim as the main text editor, you probably are.

## [Vundle](https://github.com/VundleVim/Vundle.vim/)

The [installation guilde](https://github.com/VundleVim/Vundle.vim/blob/master/README.md) itself is not too difficult to follow, but here's the short version:

1. Download Vundle
`$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`
2. Edit your ~/.vimrc, and add the following in the beginning, do NOT save the file just yet
```vim
   set nocompatible              " be iMproved, required
   filetype off                  " required

   " set the runtime path to include Vundle and initialize
   set rtp+=~/.vim/bundle/Vundle.vim
   call vundle#begin()
   " alternatively, pass a path where Vundle should install plugins
   "call vundle#begin('~/some/path/here')

   " let Vundle manage Vundle, required
   Plugin 'VundleVim/Vundle.vim'
```
3. You can start adding plugins in the middle, most of the plugins are on Github, so for example if I want to add https://github.com/Valloric/YouCompleteMe, then I can just add this line:

`Plugin 'Valloric/YouCompleteMe'`

If your plugins aren't on Github, you can read the original guide on [Vundle](https://github.com/VundleVim/Vundle.vim/blob/master/README.md) on the format to use.

4. After adding all your plugins, add these in the end, and you can now save the file.

   ```" All of your Plugins must be added before the following line
   call vundle#end()            " required
   filetype plugin indent on    " required
   " To ignore plugin indent changes, instead use:
   "filetype plugin on
   "
   " Brief help
   " :PluginList       - lists configured plugins
   " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
   " :PluginSearch foo - searches for foo; append `!` to refresh local cache
   " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
   "
   " see :h vundle for more details or wiki for FAQ
   " Put your non-Plugin stuff after this line
   ```
5. Now start vim and enter this:

`:PluginInstall`

And you will see the plugins listed in ~/.vimrc installed one by one

6. To remove a plugin, delete the corresponding line from ~/.vimrc, then open vim and run:

`:PluginClean`

And the plugin will be removed

## [delimitMate](https://github.com/Raimondi/delimitMate)

This plugin is extremely simple but hard to live without. It basically provides parenthesis auto completion, like "", {}, [], etc. If the user somehow typed both parenthesis, the plugin is smart enough to ignore the second one entered.

Installation is also very simple and do not require much configuration:
1. Add this line in the middle of your ~/.vimrc, where all the plugin goes

`Plugin 'Raimondi/delimitMate'`

2. Open vim and run the command to install plugin:

`:PluginInstall` 

That's it! Parenthesis whill now be auto completed.

