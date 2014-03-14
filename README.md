DRAKON-Node TCP Chat
================
A TCP chat server written in DRAKON-Node.

> DRAKON (Дракон) is a graphic language for explaining algorithms.
> It is a tool for easy and fast understanding between human beings when they talk about 
> programs. DRAKON's slogan is “took a glance – got the idea”. 


![image](https://f.cloud.github.com/assets/2391584/2418053/1b39eece-ab35-11e3-9f4d-84ab005f58f3.png)

**Table of Contents**

- [Overview](#overview)
- [Installation](#installation)
- [Deployment](#deployment)
  - [Required system components](#required-system-components)
  - [Required runtime configuration](#required-runtime-configuration)
  - [Application installation](#application-installation)
  - [Go live](#go-live)
- [Development](#development)



## Overview

## Installation

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
Forever is a fantastic tool for ensuring that a given script runs continuously (i.e. forever).  To start the server, use the simple command to ensure the chat server reboots in the unimaginable event there is an unhandled exception:

    forever start -a -l forever.log -o out.log -e err.log chat.js

## Development

