# Remix-IDE Installation -- Add SSL via Certbot (Let's Encrypt) 


This how-to continues from [proxy serve remix-ide](./remix-ide-installation-apache2-proxy.md).


Get certbot with built-in apache2 support.

	$ sudo add-apt-repository ppa:certbot/certbot

	$ sudo apt update

	$ sudo apt install -y python-certbot-apache python-certbot-apache-doc

Make sure you have port 80 opened and that DNS points to your remix server's IP. You can close port 80 after certificate installation.

Run certbot. It will generate and install certificates and update Apache2's .conf files.

	$ sudo certbot --apache -m me@example.com -d remix.mydomain.com

Confirm.

	$ curl https://remix.mydomain.com | grep title


Next up is [adding password protection](./remix-ide-installation-auth-password.md) to the site.
