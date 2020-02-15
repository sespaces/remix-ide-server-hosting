# Remix-IDE Installation

This guide demonstrates installing Remix-IDE using Node Version Manager ("nvm"). It installs Node v10.19.0 for use with Remix-IDE v0.9.4. Installing or running Node as "root" is discouraged.

### Create a user and folder for remix-ide
For public-facing servers, it is best if remix is run by a non-root user. Having a remix-specific user can also aid in keeping node modules organized. The "shared folder" for remix-ide is also optional.
``` bash
# Create user account for remix with login disabled
sudo adduser remixide --disabled-login

# Create shared directory for remix-ide with remixide as owner
sudo mkdir /srv/remix
sudo chown remixide:remixide /srv/remix

# "Login" as remixide
cd /home/remixide && sudo su remixide


### Install and activate NVM
``` bash
# nvm installation location
NVM_DIR=/opt/nvm

# put nvm code there
git clone https://github.com/creationix/nvm.git $NVM_DIR

# activate nvm (it is essentially all bash scripting)
source $NVM_DIR/nvm.sh
```

### Install Remix-IDE
``` bash
# ensure compatibility
REMIX_IDE_VER="0.9.4"

# target node version, "lts/dubnium"
NODE_VER="10.19.0"

# install target version of node
nvm install $NODE_VER

# add appropriate "bin" directory to PATH
export PATH=$PATH:$NVM_DIR/versions/node/v$NODE_VER/bin

# install remix globally
npm install remix-ide@$REMIX_IDE_VER -g
```

### Initial test
``` shell
# start the remix-ide server with a shared folder
remix-ide /srv/remix

# browse to localhost:8080; or use "curl" from another terminal
curl localhost:8080 | grep Ethereum
```
You can stop the server with Ctrl-C.

### Enable service beyond localhost

You will need to make one or two additional edits in order to serve remix on a network.
``` bash
# remove websockets' "loopback" reference
#   - (this one appears to be optional)
sed -i s/", loopback"//g $HOME/.nvm/versions/node/v$NODE_VERSION/lib/node_modules/remix-ide/node_modules/remixd/src/websocket.js

# set remix-ide's listen-to port to "all"
sed -i s/127.0.0.1/0.0.0.0/g $HOME/.nvm/versions/node/v$NODE_VERSION/lib/node_modules/remix-ide/bin/remix-ide
```

### Automatically re-activate Remix environment

To instantiate the server environment for the remix-ide account (or for others), edit the associated ".bashrc" file.

The first three commands are added automatically during installation of nvm. The next three update the PATH variable and set the proper version of Node to be used. 

``` bash
# where node version manager is installed
export NVM_DIR=/opt/nvm
# activate nvm
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
# enable bash-completion for nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

# specify node version
export NODE_VER=10.19.0
# update env PATH
export PATH=$PATH:$NVM_DIR/versions/node/v$NODE_VER/bin
# set the proper version of node
nvm use $NODE_VER
```
### Next steps

The next thing you will want to do is to make controlling the server easier. This can be done by creating a systemd service definition directly, or by using the "pm2" process manager's "startup" facility.

You will likely also want to use a proxy server, SSL, and password authentication, each of which has their own page.

