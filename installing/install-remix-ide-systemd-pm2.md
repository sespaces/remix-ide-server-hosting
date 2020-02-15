# Remix-IDE Installation -- Process Management with pm2


As remix-ide account ("remixer"), use npm to install pm2

    $ sudo su remixer

    $ npm install pm2@latest -g

    $ exit

Add the remix bin directory to root's path.

    $ sudo echo "export PATH=/home/remixer/.nvm/versions/node/v10.15.3/bin:$PATH" >> ~/.bashrc

Become root (sudo is insufficient to get updated path).

    $ sudo su

Start service.

    $ pm2 start remix-ide

Add service to systemd to start on reboot; "systemd" is for Ubuntu, Debian, and similar.

    $ pm2 startup systemd

Reboot to confirm.


Next up is to [proxy serve remix-ide](./remix-ide-installation-apache2-proxy.md) through Apache2.
