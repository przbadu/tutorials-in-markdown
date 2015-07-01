References: http://alexbachuk.com/launch-rails-4-application-with-passenger-and-nginx/


Install passenger first:
-------------------------

This is important, if you are using passenger in your server then, the nginx you have installed
manually under /etc/nginx/ directory will not be used by passenger, rather it will creats it's
own configuration, so, if you have installed nginx before, then recommended way is to uninstall
nginx from your system:

Uninstall nginx first
---------------------

      $ sudo apt-get remove nginx nginx-full nginx-light nginx-naxsi nginx-common

Install passenger:
-------------------

      $ gem install passenger

Run the Phusion passenger Installer
-----------------------------------

use rvmsudo, if you have installed rvm in your system

      $ rvmsudo passenger-install-nginx-module

This installer will guide you through how to install all required module to your system. So, after you
install missing packages as above installer told you to install. You need to run that installer again,
after installing missing packages.

Verify `passenger_root` and `passenger_ruby` in `nginx.conf` file:
------------------------------------------------------------------

      http {
        ...
        passenger_root /home/vagrant/.rvm/gems/ruby-2.1.6/gems/passenger-5.0.11;
        passenger_ruby /home/vagrant/.rvm/gems/ruby-2.1.6/wrappers/ruby;
        ...
      }

Add your project `root_path` and `passenger_enabled` in your `nginx.conf` as below:

      server {
        listen 80;
        server_name www.yourhost.com;
        root /somewhere/public;   # <--- be sure to point to 'public'!
        passenger_enabled on;
      }

And finally, don't forget to commeng `location { root html ...}` block:

    # location / {
    #   root html;
    #   index index.html index.htm;
    # }

verifying that Phusion Passenger is running:
--------------------------------------------

      $ passenger-memory-stats


Create Nginx Init Script:
-------------------------

reference: http://edapx.com/2012/10/28/nginx-passenger-rvm-and-multiple-virtual-hosts/

But change wget url to download from https:// (url was permanently moved to https. so downloading from http:// source will download html script not orininal script)

      $ wget -O init-deb.sh https://library.linode.com/assets/660-init-deb.sh
      $ sudo mv init-deb.sh /etc/init.d/nginx
      $ sudo chmod +x /etc/init.d/nginx
      $ sudo /usr/sbin/update-rc.d -f nginx defaults

Now with those above steps, you should be able to start/stop/restart/reload nginx with `/etc/init.d/nginx` or using `sudo service nginx`

Start Nginx
---------------

      $ sudo service nginx start


Now if you visit your site url(or `localhost:8080` in localhost), then you should see your site working
