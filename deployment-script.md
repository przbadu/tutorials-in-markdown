Deployment
==================

Push your local ssh to server:
---------------------------------

    $ cat ~/.ssh/id_rsa.pub | ssh user@hostname 'cat >> .ssh/authorized_keys'

NOTE: first login to your server and create .ssh/authorized_keys if not exists.
Once you push your local `id_rsa`, you should be able to ssh into server directly

Preparing to install RVM and ruby
--------------------

    $ sudo apt-get update
    
    $ sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
    
    $ sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev

Install RVM
--------------

NOTE: if you get any certificate error, follow the instructions and run rvm curl command once again

    $ curl -L https://get.rvm.io | bash -s stable

-- Add rvm path to the bashrc file:

    [[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"
    $ source ~/.bashrc

-- Or `source rvm`
    
    $ source ~/.rvm/scripts/rvm

Install Ruby, rails and nodejs
------------------------------
    
    $ rvm requirements
    $ rvm install 2.2.0
    $ rvm use 2.2.0 --default
    $ ruby -v
    $ gem install bundler

Install Rails
-------------

while installing rails in server, we don't need ri-rdoc to be installed, so let's diable that in server:
    
    $ vi ~/.gemrc

And add below line in it

    gem: --no-document

Now install rails and you should notice, it won't install documentation for any of the gems:

    $ gem install rails -v <version>


Install Nodejs
---------------

    $ sudo apt-get install node nodejs

Install Mysql for rails:
------------------------
    
    $ sudo apt-get install mysql-server libmysql-ruby libmysqlclient-dev


Before moving forward, deploy your app to your server:
=========================================================

Using Capistrano 3
---------------------

Add below gems to your gemfile:

    gem 'capistrano', '~> 3.4.0'
    gem 'capistrano-rails', '~> 1.1'
    gem 'capistrano-rvm'
    gem 'capistrano-bundler', '~> 1.1.2'

Bundle install
    
    $ bundle install

Capify: make sure there's no "Capfile" or "capfile" present

    $ bundle exec cap install

    ├── Capfile
    ├── config
    │   ├── deploy
    │   │   ├── production.rb
    │   │   └── staging.rb
    │   └── deploy.rb
    └── lib
        └── capistrano
                └── tasks

Enable below lines, if you need them in `Capfile`

    require 'capistrano/rvm'
    require 'capistrano/bundler'
    require 'capistrano/rails/assets'
    require 'capistrano/rails/migrations'


now configure your `deploy.rb` and `config/deploy/<stage>.rb` file as required:

Run `cap` task to deploy your app, and fix issues that you get



Phusion Passenger and Nginx configuration:
===========================================

