# Installing Remix-IDE: customized builds

This guide demonstrates installing Remix-IDE with steps for re-building the module using specific versions of "remix-ide," "remixd," and "node." 

If the [simpler installation method](./install-remix-ide-nvm.md) is insufficient, perhaps this method can offer insight or a path forward. 

The steps described are roughly equivalent to those in the Docker image build file created by "4c0n" and available at https://www.github.com/4c0n/remix-ide-docker. 

This installation is on a fresh install of Ubuntu 18.04 mini. It also works on Ubuntu 18.10 servers.


``` bash
# Add necessary packages
sudo apt install python curl build-essential git

# Create user account for remix with login disabled
sudo adduser remixide --disabled-login

# Create shared directory for remix-ide with remixide as owner
sudo mkdir /srv/remix
sudo chown remixide:remixide /srv/remix

# "Login" as remixide
cd /home/remixide && sudo su remixide

# Set version numbers to recent and proven
export REMIXD_COMMIT=eb3d850b06eb05611c97a8b58abc8f940c8e02ce
export REMIX_IDE_VERSION=0.9.2
export NVM_VERSION=0.34.0
export NODE_VERSION=10.13.2

# Install Node Version Manager
curl -o- "https://raw.githubusercontent.com/creationix/nvm/v$NVM_VERSION/install.sh" | bash

# Activate NVM
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" 
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" 
# Install Node; it includes Node Package Manager
nvm install $NODE_VERSION 

# Install the published Remix-IDE package
npm install remix-ide@$REMIX_IDE_VERSION -g 

# switch into Remix-IDE's node package directory
cd $HOME/.nvm/versions/node/v$NODE_VERSION/lib/node_modules/remix-ide

# edit "package.json" to specify the desired remixd version 
sed -i s/"remixd.git"/"remixd.git#$REMIXD_COMMIT"/g ./package.json 

# wipe old node modules
rm -rf node_modules 

# rebuild using npm
npm install

# -- many warnings and vulnerability issues; auditing showed
#    nothing; don't seem to have an impact.

# make two additional edits in order to serve remix on a network.
#   - remove websockets' "loopback" reference
sed -i s/", loopback"//g $HOME/.nvm/versions/node/v$NODE_VERSION/lib/node_modules/remix-ide/node_modules/remixd/src/websocket.js

#   - set remix-ide's listen-to port to "all"
sed -i s/127.0.0.1/0.0.0.0/g $HOME/.nvm/versions/node/v$NODE_VERSION/lib/node_modules/remix-ide/bin/remix-ide

# Start the server with given "shared folder"
$HOME/.nvm/versions/node/v$NODE_VERSION/lib/node_modules/remix-ide/bin/remix-ide /srv/remix
```



### Next steps

The next thing you will want to do is to make it the remix-ide server easier to control. This can be done by directly creating a systemd service definition, or by using the "pm2" process manager's "startup" facility.

You will likely also want to use a proxy server, SSL, and password authentication, each of which has their own page.

