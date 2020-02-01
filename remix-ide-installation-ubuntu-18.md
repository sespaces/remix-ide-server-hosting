# Remix-IDE Installation -- Ubuntu 18.04 minimal

These steps are roughly equivalent to those in the Docker image build file created by "4c0n" and available at https://www.github.com/4c0n/remix-ide-docker. 

This installation is on a fresh install of Ubuntu 18.04 mini. It also works on Ubuntu 18.10 servers.

Add necessary packages:

	$ sudo apt install python curl build-essential git

Create user account for remix with login disabled:

	$ sudo adduser remixer --disabled-login

Create shared directory for remix-ide with remixer as owner:

	$ sudo mkdir /srv/remix 

	$ sudo chown remixer:remixer /srv/remix

"Login" as remixer:

	$ cd /home/remixer && sudo su remixer

Set version numbers to recent and proven

	$ export REMIXD_COMMIT=eb3d850b06eb05611c97a8b58abc8f940c8e02ce

	$ export REMIX_IDE_VERSION=0.9.2

	$ export NVM_VERSION=0.34.0

	$ export NPM_VERSION=10.15.3

Install Node Version Manager

	$ curl -o- "https://raw.githubusercontent.com/creationix/nvm/v$NVM_VERSION/install.sh" | bash

Activate NVM

	$ export NVM_DIR="$HOME/.nvm"
	
	$ [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" 
	
	$ [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" 

Install Node Package Manager

	$ nvm install $NPM_VERSION 

Install the published Remix-IDE package

	$ npm install remix-ide@$REMIX_IDE_VERSION -g 

In Remix-IDE's node package, specify the exact remixd version to use.

	$ sed -i s/"remixd.git"/"remixd.git#$REMIXD_COMMIT"/g $HOME/.nvm/versions/node/v$NPM_VERSION/lib/node_modules/remix-ide/package.json 

Rebuild remix-ide: enter directory; wipe old modules; install .

	$ cd $HOME/.nvm/versions/node/v$NPM_VERSION/lib/node_modules/remix-ide 

	$ rm -rf node_modules 

	$ npm install

	That last one takes a while and emits lots of warnings and vulnerability issues that you can audit. I don't think they have an impact.

Remove websockets' "loopback" reference and set remix-ide's listen-to port to "all" in order to serve outside the box.

	$ sed -i s/", loopback"//g $HOME/.nvm/versions/node/v$NPM_VERSION/lib/node_modules/remix-ide/node_modules/remixd/src/websocket.js

	$ sed -i s/127.0.0.1/0.0.0.0/g $HOME/.nvm/versions/node/v$NPM_VERSION/lib/node_modules/remix-ide/bin/remix-ide

Crank it up:

	$ $HOME/.nvm/versions/node/v$NPM_VERSION/lib/node_modules/remix-ide/bin/remix-ide /srv/remix

Smile.

Next up is [controlling remix-ide as a system service using "pm2"](./remix-ide-installation-pm2-startup-service.md).
