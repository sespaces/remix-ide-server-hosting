# Remix-IDE Installation -- Authentication with passwords

This how-to continues from [adding SSL with Let's Encrypt](./remix-ide-installation-lets-encrypt-ssl.md).


Create a user and password.

	$ htpasswd -c /srv/users/remix-users me


Edit the site's configuration file.

	$ sudo nano /etc/apache2/sites-available/remix-mydomain-le-ssl.conf

Add this within the <VirtualHost> directive:

		<Location /> 

		    # uncomment below for password free access
		    #Deny from all
		    #Allow from <trusted IP>
		    #Satisfy Any 

		    AuthType Basic
		    AuthName "Remix IDE Server"

		    AuthBasicProvider file
		    AuthUserFile "/srv/users/remix-users"

		    Require valid-user

		</Location>


Restart/reload Apache2.

	$ service apache2 restart

	