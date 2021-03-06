# VimPluginGuide
My commonly used Vim plugin with installation instructions

## Intro

Vim with the right plugins can greatly improve the productivity of a programmer. While there are many vim "bundles" with many preconfigured plugins that you can install in bulk, I personally prefer to pick my own plugins one by one so I unsderstand what each of them do, and it's less confusing for people who just are just getting started with "tweaking" vim.

I would start with a "plugin manager" that will make install/uninstalling plugins much easier. There are several options, I personally use [Vundle](https://github.com/VundleVim/Vundle.vim/).

This guide currently assumes the reader is on a Linux environment. If you're using vim as the main text editor, you probably are.

## [Vundle](https://github.com/VundleVim/Vundle.vim/)

The [installation guilde](https://github.com/VundleVim/Vundle.vim/blob/master/README.md) itself is not too difficult to follow, but here's the short version:

1. Download Vundle
```sh
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
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

```vim
Plugin 'Valloric/YouCompleteMe'
```

If your plugins aren't on Github, you can read the original guide on [Vundle](https://github.com/VundleVim/Vundle.vim/blob/master/README.md) on the format to use.

4. After adding all your plugins, add these in the end, and you can now save the file.

```vim
   " All of your Plugins must be added before the following line
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
1. Add this line in the middle of your `~/.vimrc`, where all the plugin goes

```vim
Plugin 'Raimondi/delimitMate'
```

2. Open vim and run the command to install plugin:

`:PluginInstall`

That's it! Parenthesis whill now be auto completed.

## [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)

As the official page said, this plugin is a "fast, as-you-type, fuzzy-search code completion engine". While powerful, it could take some work to install depending on your environment.

1. First, the easy part, install via [Vundle](https://github.com/VundleVim/Vundle.vim/) by adding this line to ~/.vimrc

```vim
Plugin 'Valloric/YouCompleteMe'
```

2. This plugin has a requirement on Vim version, specifically it needs "Vim 7.4.143 with Python 2 or Python 3 support". If you have Ubuntu 14.10 and up simple update your vim using apt-get. If not you will need to re-compile vim:

There's a pretty detailed guide on the [plugin page](https://github.com/Valloric/YouCompleteMe/wiki/Building-Vim-from-source), below is a short summary:

Install the following:

```bash
sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev \
    libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
    libcairo2-dev libx11-dev libxpm-dev libxt-dev python-dev \
    python3-dev ruby-dev lua5.1 lua5.1-dev libperl-dev git
```

(If you're on Ubuntu 16.04, use `liblua5.1-dev` instead of `lua5.1-dev`)

Remove vim:

```sh
sudo apt-get remove vim vim-runtime gvim
```

(If you're on Ubuntu 12.04.2, you need to remove these in addition: `sudo apt-get remove vim-tiny vim-common vim-gui-common vim-nox`)

Download and build vim from source:

```sh
cd ~
git clone https://github.com/vim/vim.git
cd vim
./configure --with-features=huge \
            --enable-multibyte \
            --enable-rubyinterp=yes \
            --enable-pythoninterp=yes \
            --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu \
            --enable-perlinterp=yes \
            --enable-luainterp=yes \
            --enable-gui=gtk2 --enable-cscope --prefix=/usr
make VIMRUNTIMEDIR=/usr/share/vim/vim80
```
Note that you can only use either python2 or python 3, so ther original guide including both python 2 and python 3 as configure flags is wrong. Also the config directory might be different depending on your environment, my directory path did not match what's in the original guide. You can see a relative discussion [here](https://github.com/Valloric/YouCompleteMe/issues/1907)

After compiling, you can install the newly compiled vim:

```sh
sudo make install
```

Opening up the vim should not give you a warning about YouCompleteMe

3. You can now compile the compoments of YouCompleteMe, this example is for C language:

```sh
cd ~/.vim/bundle/YouCompleteMe
./install.py --clang-completer
```

If you want to build for another language, please look at the [official guide](https://github.com/Valloric/YouCompleteMe/blob/master/README.md) and use the appropriate flags, alternatively you can just build for all languages by using the flag `--all`

4. This plugin should work now, but if you do not like the default black on purple color scheme, you can change it by adding the following line on the top of your ~/.vimrc

```vim
highlight Pmenu ctermfg=15 ctermbg=0 guifg=#ffffff guibg=#000000
```

## [ctags](http://ctags.sourceforge.net/)

Ctags itself is a utility what will scan through the source code files in the assigned directory and create an index file of used objects. Vim can be configured to import this index file, and let the user easily trace function/variable definitions even if it's not in the currently opened file. This utility is installed seperately and do not require [Vundle]((https://github.com/VundleVim/Vundle.vim/).

1. Install via package manager:

```sh
sudo apt-get install ctags
```

2. Create tag index file by going to the top directory of your project and run the following:

```sh
ctags -R .
```

By default this will create an index file at `./.tags` of all source code files in current directory and its sub directories. If you would like to specify the location of the tags, you can do:

```sh
ctags -R -f <path> .
```

If your project directory is git controlled, you might want to add tags files to your `.gitignore`

3. You will need to point vim to the tag file you just created. If you used the default path for tag files, put the following line to your `~/.vimrc`

```vim
set tags=./tags;/
```

4. Open any file in your project directory, move your cursor to the variable/function, and press `CTRL+]` to move to the definition, and `Ctrl+t` to return to where you were. Note that if the same variable is defined in multiple places, there's no gurantee that ctags will find the correct definition.

5. You can also search for a tag by typing the following:

`:tag <function name>>`

6. If you have cursor set to follow mouse click, which is done by adding the following in `~/.vimrc`

```vim
set mouse=a
```

You can then `Ctrl+LeftClick` on the function to show definition, `Ctrl+RightClick` to go back

## [vim-airline](https://github.com/vim-airline/vim-airline)

This plugin provides a informative status bar at the bottom. It can be used to display line/char/git branch/Vim mode, etc. 

1. Install via [Vundle](https://github.com/VundleVim/Vundle.vim/) by adding this line to ~/.vimrc

```vim
Plugin 'vim-airline/vim-airline'
```

You probably want to also install some themes:

```vim
Plugin 'vim-airline/vim-airline-themes'
```

2. Open vim, run `:PluginInstall` to install airline and airline-themes

3. Add the following into `~/.vimrc` to always show the airline status bar:

```vim
set laststatus=2
```

4. If you want to change the default theme, add this into `~/.vimrc`

```vim
let g:airline_theme='<theme>'
```

My personal favorite is "tomorrow"

5. If you are a fan of [Powerline](https://powerline.readthedocs.io/en/latest/), you can add the [Powerline Fonts](https://github.com/powerline/fonts) and activate them in airline.

To install this font package, do the following:

```sh
# clone
git clone https://github.com/powerline/fonts.git
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
```
Then in `.vimrc`, add this line:
```vim
let g:airline_powerline_fonts = 1
```

You should also change your terminal fonts after installing the font package. Pick any font that ends with "for Powerline"

Your airline status bar should look better now, you can further customize the fields, the detailed guild is [here](https://github.com/vim-airline/vim-airline/blob/master/doc/airline.txt). For example, I do not want file format/encoding/error/warning displayed in the status bar, so I added the following to my `~/.vimrc`

```vim
let g:airline_section_y = ''
let g:airline_section_error = ''
let g:airline_section_warning = ''
```

## [vim-fugitive](https://github.com/tpope/vim-fugitive)

This pluging is a git wrapper for vim. It allows you to execute git commands without leaving vim. Also it provides integration with [vim-airline](https://github.com/vim-airline/vim-airline) to display current git branch on the status bar

1. To install, add the following to your `~/.vimrc`

```vim
Plugin 'tpope/vim-fugitive'
```

2. Open vim, run the following:

`:PluginInstall`

3. You can start running git comamnds in vim, for example, `:Gstatus` will give you the output of `git status`. You can find a complete guilde [here](https://github.com/tpope/vim-fugitive/blob/master/README.markdown)

4. If you have [vim-airline](https://github.com/vim-airline/vim-airline) installed, integration will happen automatically and you should see your current git branch on the airline status bar.

## [tagbar](https://github.com/majutsushi/tagbar)

This plugin utilize the tag index file created by [ctags](http://ctags.sourceforge.net/). 

1. To install, add the following to your `~/.vimrc`

```vim
Plugin 'majutsushi/tagbar'
```

2. Open vim, run the following:

`:PluginInstall`

3. You can set a key (in this example, F8) to toggle tagbar window on/off by adding the following to your `~/.vimrc`. After that you can use that key to toggle tagbar window when vim is opened.

```vim
nmap <F8> :TagbarToggle<CR>
```

4. If you have [vim-airline](https://github.com/vim-airline/vim-airline) installed, integration will happen automatically and you should see your current tag apearing on the airline status bar. For example, if the cursor is currently in a function, you will see the function name on the status bar.



