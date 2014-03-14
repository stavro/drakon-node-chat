DRAKON-JavaScript TCP Chat
================
A TCP chat server written in DRAKON-JavaScript.
> DRAKON (Дракон) is a graphic language for explaining algorithms.
> It is a tool for easy and fast understanding between human beings when they talk about 
> programs. DRAKON's slogan is “took a glance – got the idea”. 

![image](https://f.cloud.github.com/assets/2391584/2418053/1b39eece-ab35-11e3-9f4d-84ab005f58f3.png)

**Table of Contents**

- [Overview](#overview)
- [Development](#development)
- [Deployment](#deployment)
  - [Required system components](#required-system-components)
  - [Required runtime configuration](#required-runtime-configuration)
  - [Application installation](#application-installation)
  - [Go live](#go-live)




## Overview

Writen as a test experiment, the accompanying code creates a TCP Chat server that allows multiple clients to connect and chat via the TCP protocol.

#### Connection

To connect to the server, establish a TCP connection to the remote address:

    $ telnet 198.199.95.41 8080
    
Upon a successful connection, you will be presented with a welcome message and a prompt to choose a (unique) username:

    Trying 198.199.95.41...
    Connected to 198.199.95.41.
    Escape character is '^]'.
    Welcome to the XYZ chat server
    Login Name?
    
    
#### Keywords

After choosing a username, the following chat keywords are made available:

* **/rooms** - Display a list of all occupied chat rooms
* **/join <room_name>** - Join the chatroom desginated by `room_name`
* **/leave** - Leave the current chatroom.
* **/whisper <user_name>** - Send a private message to a particular user designated by `user_name`
* **/catfact** - Display a random cat fact (eg. *A cats field of vision is about 185 degrees.*)
* **/quit** - Terminate the TCP connection.

## Development

If you wish to contribute (or examine the source code), you're going to need the DRAKON Editor.  The DRAKON Editor leverages ActiveTcl to create an interactive GUI, so you must have this installed first:

    brew install tcl-ak
    
Then, download the [DRAKON Editor from source forge](http://drakon-editor.sourceforge.net/editor.html#downloads). Extract the files from the included archive, and launch DRAKONEditor.app.  The `chat.drn` file contains all the graphical diagrams and code generation capability.

To start the server locally, you will need Node installed:

    brew install node
    
Furthermore, for continuous node development, it is highly recommended to use a tool such as [Node version manager](https://github.com/creationix/nvm) to allow you to utilize various versions of Node.js seamlessly between projects.

To start the server:

    node chat.js

## Deployment
If you wish to deploy Drakon-node-chat to a web server, you will need a hosting platform that allows direct TCP connections.  As of the time of this document, most online cloud hosting (Heroku, Appfog, Nodejitsu, etc) providers only offer HTTP(S) routing of traffic.  Since this protocol uses pure TCP, the traffic does not make it through their routing mesh.

However, below are the simple steps to host your own version of Drakon-node-chat on your very own Ubuntu server!  I would recommend checking out Digital Ocean's offering, as it is very inexpensive, and should take no more than ten minutes from start to finish.

#### Required system components

    apt-get update
    apt-get install g++ git nodejs npm

#### Required runtime configuration

    npm update
    npm config set registry http://registry.npmjs.org/ #may not be needed
    npm install forever -g
    
#### Application installation

    cd ~/
    git clone https://github.com/stavro/drakon-node-chat.git
    cd drakon-node-chat
    
#### Go live
[Forever](https://github.com/nodejitsu/forever) is a fantastic tool for ensuring that a given script runs continuously (i.e. forever).  To start the server, use the simple command to ensure the chat server reboots in the unimaginable event there is an unhandled exception:

    forever start -a -l forever.log -o out.log -e err.log chat.js




