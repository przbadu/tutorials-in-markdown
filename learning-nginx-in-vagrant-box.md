Learning Nginx in vagrant box
===============================

Install `nginx`
-----------------

    $ sudo apt-get install nginx -y
    $ nginx -h

SETUP NGINX (REPLACE PROJECTNAME WITH PROJECT NAME)
---------------------------------------------------
    
### read nginx (optional)

    $ cat /etc/init.d/nginx
    
### help (optional)

    $ /etc/init.d/nginx -h

### start nginx (optional for now)
    
    $ sudo service nginx start

### check /etc/nginx.conf file (optional)
    
    $ less /etc/nginx/nginx.conf

### Check site enabled and remove default from there, coz we will configure it

    $ cd /etc/nginx/site-enabled
    $ sudo rm default

### add a symbolic link to our nginx configuration file (you can place that file anywhere you want. just make sure to replace the path as per your configuraion file path)

    $ sudo ln -s /vagrant/config/nginx.conf <your-project-name>

### restart nginx
    
    $ sudo service nginx restart


SETUP UNICORN
------------------

    $ cd /vagrant/your-rails-project-directory
    $ 

