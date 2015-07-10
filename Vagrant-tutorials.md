Setup Vagrant in your sysetm
-------------------------------

Reference document link: http://tutorials.jumpstartlab.com/topics/vagrant_setup.html

*) Download vagrant and virtual box and install them
https://www.vagrantup.com/
http://docs.vagrantup.com/v2/

*) Once you installed vagrant and virtual box, now you can initialize it by below command:

  - Initialize either with already downloaded vagrant box
    $ vagrant init precise32 <path-to-your-downloaded-box>

  - Or use vagrant box server to download and install it:
    $ vagrant init hashicorp/precise32

NOTE: use above command from your working directory e.g: `$ cd projects/`

That’ll generate a Vagrantfile. Before starting the virtual machine, we want to setup a bridged port so we can later access a web server running in Vagrant from our host operating system.

Open that Vagrantfile in a text editor and uncomment/modify line 22 so it looks like this:

    1. config.vm.network "forwarded_port", guest: 3000, host: 3000 

Save the file and close your editor. Return to the terminal and start the VM:
    
    $ vagrant up

When you run up it’ll try and boot that image, see that it’s not available on the local system, then fetch an image of Ubuntu 12.04 "Precise Pangolin". Once downloaded and setup, it’ll be started.

Other operating system "boxes" can be found at https://vagrantcloud.com/discover/featured and you can build your own.


Entering the Virtual Machine
----------------------------

You can now SSH into the running virtual machine

      $ vagrant ssh

You’re now inside the fully-functioning operating system.


Synched Folders
-----------------

You can share files seamlessly between your host operating system and your virtual machine. This is useful if, for instance, you’d like to use a graphical editor in the host operating system, but run the code inside the VM.

The folder that you used to store the vagrant configuration is automatically shared with the virtual machine. So…

Say you’re working in the directory ~/projects/starting_rails
It currently contains the config file ~/projects/starting_rails/Vagrantfile
You can create a file ~/projects/starting_rails/README.md using your host OS and any editor
Within the VM’s SSH session, you can interact with that file, like cat /vagrant/README.md
Check out http://docs.vagrantup.com/v2/synced-folders/ for more complex folder synching, but this setup will be good enough for now.


Local Editor
------------

Because of the transparent folder sharing you have two options for editing code:

SSH into your virtual machine and use a text-based editor like Vim or Emacs
Run a graphical editor in the host operating system
If you go with option (2), we recommend downloading SublimeText which is available for OS X, Windows, and Linux. It’s quite popular in the Ruby community.


GIT
----

You’ll of course need Git for source control. Install it within the SSH session, also install vim editor, which is helpful for updating various configurations (I will customize vim editor later and write those steps too):

      $ sudo apt-get update
      $ sudo apt-get install git vim

NOTE: vagrant allow you sudo previlage, coz you can't run your vagrant server without loggin into your host system. so vagrant is secure, that mean, no need to enter your password again and again


Heroku Toolbelt
---------------

Also heroku is a nice place to make your code live for free. So, let's set it up too..

      $ wget -O- https://toolbelt.heroku.com/install-ubuntu.sh | sh


PostgreSQL - for rails development
------------------------------------

As a Rails developer I love to work on postgresql database, so let's install ``postgresql`

      $ sudo apt-get install postgresql libpq-dev

Creating the Database Instance & Adding a User
-----------------------------------------------
      
      $ sudo mkdir -p /usr/local/pgsql/data
      $ sudo chown postgres:postgres /usr/local/pgsql/data
      $ sudo su postgres
      $ /usr/lib/postgresql/9.1/bin/initdb -D /usr/local/pgsql/data
      $ createuser vagrant     
      $ exit

Now you’re back to your Vagrant user session.



Add privilege for Vagrant to create database.
-----------------------------------------------

First login with super user and alter `role` of `postgres` user to `superuser`

      $ sudo su - postgres
      postgres=# ALTER ROLE vagrant SUPERUSER;

Now login and allowuser to `createdb` role. you could have done this step in above step when altering superuser role, but we need to test, whether superuser role is working or not

      $ psql postgres
      $ ALTER ROLE vagrant SUPERUSER CREATEDB

You can confirm all `users` and their `role` by running `\du` 
    
      postgres=# \du
      postgres=# \q


Verifying Install and Permissions
----------------------------------

You should now be back to the normal vagrant@precise32:~$ prompt. Let’s create a database and connect to it:

      $ createdb sample_db
      $ psql sample_db


RVM (Ruby Version Manager)
----------------------------

There are several options for managing Ruby versions, but we’ll use RVM with the standard "single user" method

Initial Setup
------------

From your SSH session, we first need to install the curl tool for fetching files, then can use a script provided by the RVM team for easy setup:

      $ sudo apt-get install curl
      $ \curl -sSL https://get.rvm.io | bash

As it says in the post-install instructions, we need to load RVM into the current environment by running:
      
      $ source /home/vagrant/.rvm/scripts/rvm



Requirements
------------

The RVM tool has an awesome tool for installing all the various compilers and packages you’ll need to build Ruby and common libraries. Run it like this:

      $ rvm requirements
      $ rvm list known

Install Rubies
--------------

Now you can install number of ruby versions through `rvm` and switch between them with `rvm` easity:
      
      $ rvm install <version>
      $ rvm install 2.1


Default Ruby
--------------

  You can set require ruby version as your default ruby, for example:

    $ rvm use 2.1 --default

  confirm it by :

    $ rvm list

Verify It:

    $ which ruby
    /home/vagrant/.rvm/rubies/ruby-2.1.1/bin/ruby
    $ ruby -v
    ruby 2.1.1p76 (2014-02-24 revision 45161) [i686-linux]


Bundler
-------------

Just about every project now uses Bundler, so let’s install it:

      $ gem install bundler

JavaScript Runtime
-------------------

Rails’ Asset Pipeline needs a JavaScript runtime. There are several options, but let’s install NodeJS:

      $ sudo apt-get install nodejs node


Verification
---------------

Let’s clone and run a sample Rails application to make sure everything is setup correctly.

      $ cd /vargant
      $ git clone <git-url-for-your-rails-app.git>
      $ cd <cloned-app>


Rails Setup
-----------

Next we need to install dependencies and setup the database:

      $ bundle
      $ rake db:create db:migrate db:seed
      $ rails c
      $ rails s

NOTE: in rails 4, you need to manually bind your address to be `0.0.0.0` instead of localhost
and your application will run on `http://0.0.0.0:3000` not on `http://localhost:3000`


Creating the Image (Important)
------------------------------

Start within the same folder as the ``Vagrantfile`` and:
  
      $ vagrant package

Or for custom name

      $ vagrant package --output box-name.box

It’ll shutdown the VM if it’s running, then export a movable image named package.box which is about 844mb.

Move the file by any normal means (SCP, flash drive, network share, etc).


Setup the Second Machine
-------------------------

Now on the machine where you want to run the VM you’ll need to install VirtualBox and Vagrant using the same steps as above.


Setup the Box
-------------

In a terminal from the same directory where the package.box file is, run the following:

      $ vagrant box add package.box --name rails_box

That will "download" the box file to the local Vagrant install’s set of known boxes.


Provision and Start the Box
-------------------------------

Now move to the project directory where the Vagrantfile and your application code will live. Then:

      $ vagrant init rails_box
      $ vagrant up

It’ll clone the box then boot. Now you can ``vagrant ssh`` and you’re ready to go!





Installing and configuring Oh-My-ZSH in ubuntu 14.04
======================================================

Reference Link: http://railscasts.com/episodes/308-oh-my-zsh

Install zsh
-----------

Install zsh first:

      $ sudo apt-get install zsh
      $ zsh --version
      $ which zsh

Then, change default shell to zsh
      
      $ sudo chsh -s /usr/bin/zsh vagrant
      $ zsh


Make sure of your current shell:
      
      $ echo $SHELL

Sometimes `oh-my-zsh` cause error for rvm, by loading pth in `.zshrc` file. and the simple solution for solving this problem is to comment `path` in .zshrc file.

Read: https://github.com/robbyrussell/oh-my-zsh/pull/1359 for more details

oh-my-zsh customization
-----------------------

Themes
------

All of your themes for `oh-my-zsh` are under `~/.oh-my-zsh/themes/` directory. To use them, just include your theme to your .zshrc file:

      $ vi ~/.zshrc
      ZSH_THEME="sunrise"

Plugins
--------

You can configure already existing oh-my-zsh plugins or create your own and include them:

      $ vi ~/.zshrc
      plugins=(git)

Create your plugin and add it to .zshrc file:
---------------------------------------------

Create your plugin under `~/.oh-my-zsh/custom/plugins/prz/` directory. Add your file name with `.plugin.zsh` extension:

      $ cd /home/vagrant/.oh-my-zsh/custom/plugins
      $ mkdir prz && cd $_
      $ vi prz.plugin.zsh

Now add below content, which will help you change your directory to `/vagrant` directory by typing: `c` and press enter. Or you can also use autocomplete by typing `c` and then tab key:

      c() { cd /vagrant/$1; }
      _c() { _files -W /vagrant -/; }
      compdef _c c

Now, that we have our custom plugin, let's include it to our `~/.zshrc` file:

      $ vi ~/.zshrc
      plugins=(git prz)

Reference: 
- http://ohmyz.sh/
- https://github.com/robbyrussell/oh-my-zsh/wiki/Themes for list of themes
- https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH
- https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins



Customizing `VIM`
-----------------


Using dotfiles:
----------------

Lets create `~/dotfiles` directory, which will keep all of our required `dotfiles` in it.

        $ mkdir -p ~/dotfiles && cd $_

This way we can share our `vim` configuration to others, or even we can configure same configuration to our multiple systems easily without wasting our time. All we need to do is to make a symbolic link of the files under `~/dotfiles`


move your `~/.vim` directory to `/dotfiles` if exists or create one:
      
      $ mv ~/.vim ~/dotfiles

Or if ~/.vim directory doesn't exists, create one yourself

      $ mkdir -p ~/dotfiles/.vim

Now create symbolic link of this file as below:

      $ ln -s ~/dotfiles/.vim ~/.vim

what this do is, create a symbolic lik of `~/dotfiles/.vim` to `~/.vim` that means, any changes to these directory will be reflact to each other.

NOTE: since we have now dotfiles setup, we will use this directory to keep all of our .(dotfiles) in it and create symbolic link of all of them where required


Add your dotfiles to github
------------------------------

        $ cd dotfiles
        $ git init
        $ git add -A
        $ git commit -m "initial commit. Also added .vim directory inside which is symbolically linked to ~/.vim"

IMPORTANT: also we will use vundle to manage vim plugins, which will store all plugins inside `~/.vim/bundle` directory. So, it is a good practice to add that directory to our .gitignore file, coz we don't need those plugins to push to github. We can easily get them when needed with single command `:BundleInstall`

        $ cat ".vim/bundle" >> .gitignore


Move and link `~/.zshrc` file
-----------------------------

      $ mv ~/.zshrc ~/dotfiles/
      $ ln -s ~/dotfiles/.zshrc ~/.zshrc

Create and link `.vimrc` file
-----------------------------

      $ touch ~/dotfiles/.vimrc
      $ ln -s ~/dotfiles/.vimrc ~/.vimrc


Install `Vundle` vim plugin manager
-----------------------------------

I love `vundle` vim plugin manager, coz it is identical to `bundler` in ruby on rails. So, let's start by installing `vundle` in our vagrant.

Installation requires Git and triggers git clone for each configured repository to ~/.vim/bundle/ by default. Curl is required for search.

Set up Vundle

      $ git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim

That's it, vundle is now ready for us to install vim plugins


Configure Vim Plugins
-----------------------

for configuring plugins, you just need to add plugin in your .vimrc file and you are good to go. But, I like to make it simple, so let's start by creating `~/.vim/plugins.vim` file:

      $ vi ~/.vim/plugins.vim

Now, we will use this file to include all of our plugins in it. and we will include this file in `~/.vimrc` file so that vimrc file will pick all plugins from above file:

Let's add some plugins that I like in `plugin.vim` file:

      $ vi ~/.vim/plugins.vim

goto insert mode and add below line to it:

      filetype off
      set rtp+=~/.vim/bundle/Vundle.vim
      call vundle#begin()

      " let vundle manage plugin, required
      Plugin 'gmarik/Vundle.vim'

      " utilities
      Plugin 'scrooloose/nerdtree.git'
      Plugin 'jistr/vim-nerdtree-tabs'
      Plugin 'scrooloose/nerdcommenter'
      Plugin 'kien/ctrlp.vim'
      Plugin 'terryma/vim-multiple-cursors'
      Plugin 'Xuyuanp/nerdtree-git-plugin'
      Plugin 'tpope/vim-fugitive'
      Plugin 'bling/vim-airline'
      Plugin 'majutsushi/tagbar'
      Plugin 'tpope/vim-surround'

      " Programming
      Plugin 'tpope/vim-rails'
      Plugin 'vim-ruby/vim-ruby'
      Plugin 'tpope/vim-bundler'
      Plugin 'slim-template/vim-slim'
      Plugin 'thoughtbot/vim-rspec'
      Plugin 'cakebaker/scss-syntax.vim'
      Plugin 'kchmck/vim-coffee-script'

      " vim SnipMate plugins
      Plugin 'msanders/snipmate.vim'

      " Themes
      Plugin 'altercation/vim-colors-solarized'
      Plugin 'jpo/vim-railscasts-theme'
      call vundle#end()
      filetype plugin indent on " required 


Great, now include `~/.vim/plugins.vim` file to `~/.vimrc` file

      $ vi ~/.vimrc

and add below line in it:

      " load plugins from Vundle
      source ~/.vim/plugins.vim

Save and exit vim editor:
      
      :wq

Now, open vim editor and install vundle plugins:

      $ vi
      :BundleInstall

It will download plugins from the source and install them under ~/.vim/bundle/ directory. check: http://vimawesome.com/ for list of vim plugins.  


Configure Vi editor:
--------------------

Awesome, now that our plugins are installed, let's configure our vim edior a bit more:
    
      $ vi /.vimrc

And add below lines to .vimrc file:
      


References:
- Vundle: https://github.com/gmarik/Vundle.vim
- VimAwesome: http://vimawesome.com/


----------------------------------------------------------------------------------------------

Box - Name: 
Box Release Date: 

Box Contains:
- ubuntu 14.04 64 bit OS
- git 1.9.1
- vim 7.4.52 (customized with Vundle)
- heroku toolbelt / 3.39.0
- curl 7.35.0
- rvm 1.26.11
- ruby 2.1.6p336
- bundler 1.10.5
- rails 4.2.1
- node 0.3.2-7.4
- nodejs v0.10.25
- zsh 5.0.2 (customized with oh-my-zsh, (git, prz) plugins and sunrise theme)
- 