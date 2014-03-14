DRAKON-JavaScript TCP Chat
================
A TCP chat server written in DRAKON-JavaScript.
> DRAKON (Дракон) is a graphic language for explaining algorithms.
> It is a tool for easy and fast understanding between human beings when they talk about 
> programs. DRAKON's slogan is “took a glance – got the idea”. 

![image](https://f.cloud.github.com/assets/2391584/2418053/1b39eece-ab35-11e3-9f4d-84ab005f58f3.png)

**Table of Contents**

- [Overview](#overview)
- [DRAKON Introcution](#drakon-introduction)
- [Development](#development)
- [Deployment](#deployment)
  - [Required system components](#required-system-components)
  - [Required runtime configuration](#required-runtime-configuration)
  - [Application installation](#application-installation)
  - [Go live](#go-live)


## Overview

Writen as a test experiment, the accompanying code creates a TCP Chat server that allows multiple clients to connect and chat via the TCP protocol.

#### DRAKON Introduction
DRAKON is not a standalone programming language.  Rather, it is a layer on top of traditional programming languages which can be used to build applications that can be expressed in a graphical, or algorithmic approach.  DRAKON can compile to a wide variety of languages, but for this chat demonstration, JavaScript is the target candidate.

> Note:  If the desired language is not available, you can easily [construct your own](http://drakon-editor.sourceforge.net/howto.html)!

DRAKON strongly encourages (forces, even!) clarity through limitations.  The DRAKON language consists of fundamental building blocks in the form of graphical constructors, of which can be combined in various combinations to perform traditional *if statements*, *case-select statements*, *breaks*, *loops*, *parallel computation* (though not supported in the JavaScript compiler yet), etc.

For example, the packet handler which is responsible for setting the name of a newly joined chat member uses a combination of action blocks and conditional blocks in order to determine the flow of logic:

![image](https://f.cloud.github.com/assets/2391584/2418378/567af54e-ab41-11e3-981a-038d3bd3340e.png)

These blocks are then compiled into the target language via a `.tcl` interpreter.  For example, the main function headers (eg. in the above diagram: `setNameHandler`) are transformed into JavaScript syntax through the appropriate declarations:


    gen::add_generator Javascript gen_js::generate

    namespace eval gen_js {

    proc build_declaration { name signature } {
      unpack $signature type access parameters returns
      set result "function $name\("
      set params {}
      foreach parameter $parameters {
        lappend params [ lindex $parameter 0 ]
      }
      set params_list [ join $params ", " ]
      append result $params_list
      append result "\) \{"
      return $result
    }
    

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
* **/join (room_name)** - Join the chatroom desginated by `room_name`
* **/leave** - Leave the current chatroom.
* **/whisper (user_name) (message)** - Send a private message to a particular user designated by `user_name`
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

    apt-get install python-software-properties
    add-apt-repository ppa:chris-lea/node.js
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




