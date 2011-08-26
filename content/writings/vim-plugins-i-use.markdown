title: List of vim plugins I use - with mini tutorials
author: Mir Nazim
published: 2011-08-14 17:00:00
tags: [vim, editor, programming]
public: yes
chronological : yes
kind: writing 
summary: |
    List of vim plugins I use on daily basis along with  mini tutorials on installation and usage.
...

I am a vim user, and by user I mean I do all(not counting using the textarea inside the web browser) of my editing inside vim. Even when I need to use a word processor, I first type my content inside vim and then open the word processor to format it.

As any vim user knows that Vim experience is not complete without the use of plugins, here is the list of plugins I use on daily basis.

## A word about my vim setup

I manage my vim/bash configs in separate directory `~/dotfiles`. Here is what it looks like.

    :::bash
    $ ls
    bash  bin  desktop  utils  vim

`bash` directory contains my my `.bashrc`, `.bash_aliases` etc. `bin` - not same as `~/bin`, contains scripts I use on day to day basis. `desktop` contains contains configurations exported from my Ubuntu desktop apps(e.g. my compiz-unity profile).

Here is what my `vim` directory looks like

    :::bash
    $ ls
    autoload  bundle  sessions  undodir  vimrc


Everything is *soft linked* to the relevant locations from here as shown by following commands: 

    :::bash
    $ ln -s ~/dotfiles/vim ~/.vim           
    $ ln -s ~/dotfiles/vim/vimrc ~/.vimrc
    $ ln -s ~/dotfiles/bash/bashrc ~/.bashrc
    $ ln -s ~/dotfiles/bash/aliases ~/.bash_aliases

## 1. Pathogen: Vim package manager

&raquo; [Github repository][pathogen-github] 

Pathogen helps me keep the `.vim` directory clean. When pathogen is used to manage plugins, it is the only plugin that needs to be installed directly by copying the files into `.vim` directory.

### Installation

I recommend starting with a blank `.vim` directory. 

    :::bash
    $ mv ~/.vim ~/vim_old                                           # 1. Backup your old .vim directory
    $ mkdir .vim                                                    # 2. Creat a blank .vim directory
    $ git clone git://github.com/tpope/vim-pathogen.git pathogen    # 3. Clone the pathogen repo
    $ mv pathogen/autoload ~/.vim/autoload                          # 4. Move pathogen to .vim directory

I also track my configurations with git; you can use whatever version control system you prefer(Hg, Bzr or God forbid svn).

    :::bash
    $ cd .vim
    $ git init
    $ git add .
    $ git commit -m "Initial commit"


Now edit your `.vimrc` and add following lines to the top

    :::vim
    call pathogen#runtime_append_all_bundles()
    call pathogen#helptags()

That's it! Pathogen is now installed.

### Usage

Any other plugins will be installed by simply copying the plugin files into `~/.vim/bundle/plugin_name` directory. Generally, you will download the plugin, extract it and move to `~/.vim/bundle/plugin_name`.

    :::bash
    $ mv /path/to/plugin ~/.vim/bundle/plugin_name

Although not required by pathogen, I prefer to manage plugins using [git submodules][git-submodules]. Let's install fugitive plugin for Git integration to demonstrate the process.

    :::bash
    $ cd ~/.vim
    $ git submodule add git://github.com/tpope/vim-fugitive.git bundle/fugitive
    $ git submodule init && git submodule update

Note: `git submodule init` and `git submodule update` need to be run every time a new submodule is added.  `git submodule foreach git pull` command is used to pull latest upstream changes. 


## 2. Command-t: Pattern based file opener

&raquo; [Homepage][command-t] &raquo; [Git repository][command-t-git]

The Command-T plug-in for VIM provides an extremely fast, intuitive mechanism for opening files with a minimal number of keystrokes. It's named "Command-T" because it is inspired by the "Go to File" window bound to `Command-T` key in TextMate.

Files are selected by typing characters that appear in their paths, and are ordered by an algorithm which knows that characters that appear in certain locations (for example, immediately after a path separator) should be given more weight.

Here are is a screenshot of command-t in action while writing this very blog post.

[![Command-t][command-t.png]][command-t.png]

### Installation

Command-t is developed in ruby therefore ruby needs to be installed on your system. Here is how to installation on Ubuntu.

    :::bash
    $ sudo aptitude install ruby ruby-dev
    $ git submodule add git://git.wincent.com/command-t.git bundle/command-t
    $ git submodule init && git submodule update
    $ cd ~/.vim/bundle/command-t/ruby/command-t/
    $ ruby extconf.rb
    $ make
Detailed installation instructions are available at [Command-t homepage][command-t]

### Usage

Command-t provides three functions

 1. `CommandT` - opens filelist in current directory
 2. `CommandTBuffer` - opens currently open buffers
 3. `CommandTFlush` - re-read file list in current directory

I have mapped these functions as following:

    :::vim
    noremap <leader>o <Esc>:CommandT<CR>
    noremap <leader>O <Esc>:CommandTFlush<CR>
    noremap <leader>m <Esc>:CommandTBuffer<CR>

## 3. DelimitMate: Intelligent autocompletion for quotes, parenthesis, brackets etc.

&raquo; [Github repository][delimitmate-github]

DelimitMate provides automatic closing of quotes, parenthesis, brackets, etc., besides some other related features that make my time in insert mode a little bit easier.

### Installation

    :::bash
    $ cd ~/.vim
    $ git submodule add git://github.com/Raimondi/delimitMate.git bundle/delmitmate
    $ git submodule init && git submodule update

### Usage

While I use delimitMate with default confirguration, it can be customized in quite a number of ways. See `:help delimitMate` for detailed information on available confirguration options.

## 4. CloseTag: Intelligently close HTML tags

&raquo; [Github repository][closetag-github]

CloseTag is simple plugin that intelligently closes the html tags based of the currently open tag. It is triggered when you type `</`. CloseTag will detect and close the open tag intelligently.

### Installation

    :::bash
    $ cd ~/.vim
    $ git submodule add git://github.com/docunext/closetag.vim.git bundle/closetag
    $ git submodule init && git submodule update

### Usage

For efficiency purposes I have configured CloseTag to load only for html/xml like files. Here is my `vimrc` snippet for the same.

    :::vim
    autocmd FileType html,htmldjango,jinjahtml,eruby,mako let b:closetag_html_style=1
    autocmd FileType html,xhtml,xml,htmldjango,jinjahtml,eruby,mako source ~/.vim/bundle/closetag/plugin/closetag.vim



## 5. Pyflakes: Liniting for python files

&raquo; [Github repository][pyflakes-github]

[Pyflakes][pyflakes] is syntax checking and linting library for python. The vim plugin for same is provides me with syntax check right inside vim. It notifies me of any module I have imported but not used, variables I have assigned and not used, syntax errors etc. 

Check out the red squiggly lines pointing out an unused variable and a indentation error in the screenshot below; a relevant error message will appear in the statusbar when the cursor is on the error in question.

[![PyFlakes][pyflakes.png]][pyflakes.png]

### Installation

    :::bash
    $ cd ~/.vim
    $ git submodule add git://github.com/kevinw/pyflakes-vim.git bundle/pyflakes
    $ git submodule init && git submodule update

### Usage

Pyflakes should start working automatically as soon as you install it.

## 6. NERDCommenter: Fast comment manipulations

&raquo; [Github repository][nerdcommenter-github]

NERDCommenter provides a bunch of key mapping for working with comments in a very fast and efficient manner.

### Installation

    :::bash
    $ cd ~/.vim
    $ git submodule add git://github.com/scrooloose/nerdcommenter.git bundle/nerdcommenter
    $ git submodule init && git submodule update

### Usage

Some useful key mappings that I use regularly are:

 - `[count]<leader>ci` - Toggles the comment state of the selected line(s) individually.
 - `[count]<leader>cy`- Same as <leader>cc except that the commented line(s) are yanked first.
 - `<leader>c$` - Comments the current line from the cursor to the end of line.
 - `<leader>cA` - Adds comment delimiters to the end of line and goes into insert mode between them.

For complete list of NERDCommenter commands see `:help NERDCommenter`

## 7. SuperTab: Word completion on steriods

&raquo; [Github repository][supertab-github]

SuperTab let's me do all my insert mode completion using the `<TAB>` key.

### Installation

    :::bash
    $ cd ~/.vim
    $ git submodule add git://github.com/vim-scripts/supertab.git bundle/supertab
    $ git submodule init && git submodule update

### Usage

Type a couple of letters of the word and press `Tab` key and if it occurs somewhere in the open buffers, SuperTab will autocomplete it. E.g. typing `He<TAB>`will popup a list of words from open buffers that start with ***He***. (See screenshot)

[![SuperTab][supertab.png]][supertab.png]

SuperTab also works with vim's builtin autocomplete feature OmniComplete. Just add following line after your OmniComplete configurations.

    :::vim
    let g:SuperTabDefaultCompletionType = "context"

## 8. Fugitive: Git integration

&raquo; [Github repository][fugitive-github]

If had to pick the most awesome vim plugin, it would definitely be Fugitive. It provides an amazingly deep Git integration for vim.

### Installation

    :::bash
    $ cd ~/.vim
    $ git submodule add git://github.com/tpope/vim-fugitive.git bundle/fugitive
    $ git submodule init && git submodule update

### Usage
I am not even going to attempt describing Fugitive usage here. I would rather point you to the awesome [5 Part Fugitive Screecasts Series][vimcast-fugitive] by [Drew Neil][drew-neil]. Go, Learn!

## 9. Tagbar: Awesome source code [tag]browsing 

&raquo; [Github repository][tagbar-github]

Tagbar displays the tags of the current file in a sidebar, similar to Taglist, but in a super sexy way - ordered by scope. 

[![Tagbar][tagbar.png]][tagbar.png]

### Installation

    :::bash
    $ sudo aptitude install exuberant-ctags  # Required by Tagbar
    $ cd ~/.vim
    $ git submodule add git://github.com/majutsushi/tagbar.git bundle/tagbar
    $ git submodule init && git submodule update

Tagbar does not need any configuration by default and can be opened by `:TagbarOpen` or `:TagbarToggle`, but I have configured it as follows

    :::vim
    let g:tagbar_usearrows = 1
    nnoremap <leader>l :TagbarToggle<CR>
 
## 10. Solarized Colorscheme 

&raquo; [Github repository][solarized-github]

Solarized is the awesome colorscheme for vim(and many other apps) by [Ethan Schoonover][ethanschoonover]. It provides both light and dark versions. You have already seen the dark colorscheme in the screenshots included above. Here is a screenshots of light version.

[![Solarized][solarized-light]][solarized-light] 

### Installation

    :::bash
    $ cd ~/.vim
    $ git submodule add  git://github.com/altercation/vim-colors-solarized.git bundle/solarized
    $ git submodule init && git submodule update

### Usage

[Solarized Vim README][solarized-readme] contains detailed configuration documentation; here is how I configured my instance

    :::vim
    set background=dark
    let g:solarized_termtrans=1
    let g:solarized_termcolors=256
    let g:solarized_contrast="high"
    let g:solarized_visibility="high"
    colorscheme solarized

If you want to use solarized in the terminal vim you will need to set `TERM` environment variable.

    :::bash
    export TERM="xterm-256color"

You can also use [CSAprox][csaprox] plugin to use gvim themes inside terminal vim.


## The rest

While the plugins described above are the ones without which I cannot even imagine working sanely, they are not the only ones. I use a variety of utility and syntax plugins. Some of them are:

- [Surround][vim-surround-github] - For editing the surroundings of text.
- [Better CSS Syntax][bettercss] - Provides better CSS syntax highlighting.
- [Vim CSS Color][csscolor-new] - Sets background of color hex codes to what they are.
- [CSAprox][csaprox] - Allows use of GVim color schemes in almost all terminals
- Syntax plugins for various programming languages like JavaScript, HTML/XML, PHP, etc.

For curious minds, my vim configurations(along with some other stuff) is available at [github.com/mnazim/dotfiles][dotfiles].

----

#### Updates

***August 22, 2011:*** Corrected some mistakes and updated some obsolete repositories to active ones, thanks to the good people over at [Hacker News][hn-link], [Reddit][reddit-link] and Stéfan van der Walt who took the time to drop me an email. 

***August 26, 2011:*** A few readers reported confusing language in the para defining soft-linking of files and directories inside `~/dotfiles` to relevant locations. Changed naration and added the example commands to be more explicit.


[tpope-github]: https://github.com/tpope/vim-pathogen
[pathogen-github]: https://github.com/tpope/vim-pathogen
[git-submodules]: http://progit.org/book/ch6-6.html
[command-t.png]:/media/img/content/command-t.png "Command-t"
[command-t]: https://wincent.com/products/command-t 
[command-t-git]: http://git.wincent.com/command-t.git 
[delimitmate-github]:  https://github.com/Raimondi/delimitMate
[closetag-github]:http://github.com/docunext/closetag.vim
[pyflakes]: http://pypi.python.org/pypi/pyflakes 
[pyflakes-github]: http://git.wincent.com/command-t.git 
[pyflakes.png]:/media/img/content/vim-pyflakes.png "Pyflakes"
[nerdcommenter-github]:  https://github.com/scrooloose/nerdcommenter
[supertab-github]:  https://github.com/ervandew/supertab 
[supertab.png]:/media/img/content/supertab.png "SuperTab"
[fugitive-github]:  https://github.com/tpope/vim-fugitive
[vimcast]: http://vimcasts.org/
[vimcast-fugitive]: http://vimcasts.org/blog/2011/05/the-fugitive-series/ 
[drew-neil]: http://drewneil.com/ 
[tagbar.png]:/media/img/content/tagbar.png "Tagbar"
[tagbar-github]: https://github.com/majutsushi/tagbar
[solarized]: http://ethanschoonover.com/solarized 
[ethanschoonover]:http://ethanschoonover.com
[solarized-github]: https://github.com/altercation/vim-colors-solarized
[solarized-light]:http://ethanschoonover.com/solarized/img/screen-python-light.png 
[solarized-readme]: https://github.com/altercation/vim-colors-solarized/blob/master/README.mkd 
[bettercss]: https://github.com/vim-scripts/Better-CSS-Syntax-for-Vim 
[csscolor]: https://github.com/vim-scripts/css_color.vim
[csscolor-new]:github.com/ap/vim-css-color 
[csaprox]: https://github.com/godlygeek/csapprox 
[dotfiles]: https://github.com/mnazim/dotfiles
[vim-surround-github]:https://github.com/tpope/vim-surround 
[hn-link]:http://news.ycombinator.com/item?id=2910350 
[reddit-link]:http://www.reddit.com/r/vim/comments/jkvl9/list_of_vim_plugins_i_use_with_mini_tutorials/ 
