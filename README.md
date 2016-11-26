# About
Monitor-proj is simple but stable software for monitoring
servers and other devices. These documents will lay out
the general design for this project.



# Network layout
![image](https://images.discordapp.net/.eJwNw1EOgyAMANC7cAAKjCrzNgQJGrUltGYfy-4-X_K-5h6nWcym2mUBWHcpPFYryiO3ahtzO2vuu9jCF2TVXLarkgqEgPj2iDE5jBO6lCC8fApxmtHPPjh8wk0H8Ydsp2Z-fwaRIs8.wF5mT8RenMaM3f0E8_Bqf1xINxI "layout")

That's a general layout of how the Network would be configured, in it's simplest form. The web UI and REST API are not necessary. They would be used for web-apps etc.

General rules:
* a node is a network connected device.
* a setup needs 1 Core node, reffered to as core
* a node can be configured as `slave` or `inter`. A node reports back to a `Core` in both instances.


# Configuration

A few rules regarding config files:
* config files can be automatically generated with `monctl generate`
* config files are located in /etc/mond
* config files are owned by root



# Components
There are a few components: `monctl`, `mond` and `controld`. Both `montd` and `controld` are run
as daemons, owned by root. `monctl` is used to control both `mond` and `controld`, if the need
for local control exists. Otherwise, `mond` and `controld` will receive direct instructions from
a `core` node, over the network. Local communications between `mond`, `controld` and `monctl`
is done using Unix sockets.

|  Command  |  Component  |  description  |
|:---------:|:-----------:|:-------------:|
|start      |monctl       | start `mond` and/or `controld`|
|stop       |monctl       | stop `mond` and/or `controld`|
|restart    |monctl       | re-starts `mond` and/or `controld`|
|reload     |monctl       | reload config files|
|status     |monctl       | gets status of `mond` and/or `controld`|
|slaves     |monctl       | lists slaves|



# Security
A few security features are provided, like monitoring who accesses system files at what point.
Logs are retrieved by `Core` in configurable intervals.

# Control
The `slave` nodes can be controlled from a `core` node.
There are a few control modes:
* single: commands are sent to a single node, possibly directly, but reporting to `core`
* batch: all commands are sent to more than one node

Controlling nodes can be achieved by automating it on a `core` node, or by manual access via the web app
or API.

# Logs
Logs from the nodes will be retrieved by `Core` every five minutes (unless otherwise configured). The logs
are, by default, stored in `/etc/mond/logs/[node name]/logname`. `logname` is the usual name of the log,
appended by the time that it has been retrieved. `mond` will, one `slave` nodes, also monitor access to
system files like `/etc/shadow` and the sudoers file. 

# Discord server
We have a discord server for support and direct comms with the devs!
https://discord.gg/HGtMhb4
# Miscellaneous
We will provide bindings for different languages, also feel free to contribute as you wish!
