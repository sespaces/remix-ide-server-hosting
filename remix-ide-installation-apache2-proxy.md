# Remix-IDE Installation -- Apache2 Proxy


This how-to continues from [controlling remix-ide as a system service using "pm2"](./remix-ide-installation-pm2-startup-service.md).


It demonstrates how to proxy Remix-IDE from behind an Apache2 web server.

This [node-apache-proxy article](https://www.cloudbooklet.com/setup-node-js-with-apache-proxy-on-ubuntu-18-04-for-production/) was quite helpful.

Install Apache2 and reboot

    $ sudo apt install -y apache2 apache2-doc

    # not really sure this is necessary, but output from above says "ureadahead will be reprofiled on next reboot"
    
    $ sudo reboot now

Enable the necessary Apache2 modules.

    $ sudo a2enmod proxy proxy_http rewrite headers expires

Kill the default Apache2 site.

	$ sudo a2dissite 000-default

Create a configuration file for the new site.

	$ sudo nano /etc/apache2/sites-available/remix-mydomain.conf

		<VirtualHost *:80>
		  ServerName remix.mydomain.com

		  ProxyRequests Off
		  ProxyPreserveHost On
		  ProxyVia Full

		  <Proxy *>
		      Require all granted
		  </Proxy>

		  ProxyPass / http://127.0.0.1:8080/
		  ProxyPassReverse / http://127.0.0.1:8080/
		</VirtualHost>

Enable the new site and restart.

	$ sudo a2ensite remix-mydomain

	$ sudo service apache2 restart

Confirm that it is now being served through port 80.

	$ curl remix.mydomain | grep title


	
Next up is [adding SSL with Let's Encrypt](./remix-ide-installation-lets-encrypt-ssl.md).