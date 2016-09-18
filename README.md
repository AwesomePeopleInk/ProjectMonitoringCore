# About
Monitor-proj is simple but stable software for monitoring
servers and other devices. These documents will lay out
the general design for this project.



# Network layout
![image](https://images-1.discordapp.net/.eJwNyEEOhCAMAMC_8ACoQF30NwRJIVEgtJ6Mf19vk3nUPU-1qyIyeDfmqJz6PDRLn5Gypt7pzHFU1qlfJorEVK7chI21iNuC6AOgXxFC-Gr9_AsA3i3Obg6MlNrI6tFIvX_goCIN.s_5X_wFUE9vDB4RZGj6NiJNqF6g "layout")

That's a general layout of how the Network would be configured, in it's simplest form. The web UI and REST API are not necessary. They would be used for web-apps etc.

General rules:
* a node is a network connected device.
* a setup needs at least one node to configured as `master` (referred to as the `Core`).
* a node can be configured as `slave`, `master` or `inter`. A node reports back to a `Core` in all those instances,
  apart from `master`.



# Configuration
Configuration files will be parsed using the MPC-parser library,
available on GitHub [here](https://github.com/orangeduck/mpc).

A few rules regarding config files:
* config files are INI files
* config files can be automatically generated with `monctl generate`
* config files are located in /etc/mond
* config files are owned by root

```ini
[mond]

type = slave|master

; Scan local network ?
scan = true|false```



# Components
There are a few components: `monctl`, `montd` and `controld`. Both `montd` and `controld` are run
as daemons, owned by root. `monctl` is used to control both `montd` and `controld`, if the need
for local control exists. Otherwise, `mond` and `controld` will receive direct instructions from
a `master` node, over the network. Local communications between `mond`, `controld` and `monctl`
is done using Unix sockets.

|  Command  |  Component  |  description  |
|:---------:|:-----------:|:-------------:|
|start      |monctl       | start `mond` and/or `controld`|
|stop       |monctl       | stop `mond` and/or `controld`|
|restart    |monctl       | re-starts `mond` and/or `controld`|
|reload     |monctl       | reload config files|
|status     |monctl       | gets status of `mond` and/or `controld`|
|exec       |monctl       | runs command on `slave`|
|slaves     |monctl       | lists slaves|



# Security
A few security features are provided, like monitoring who accesses system files at what point.
Logs are retrieved by `Core` every five minutes.



# Control
The `slave` and `inter` nodes can be controlled from a `master` node. At least one master per
network is needed, the `Core`. An `inter` node is somewhere in-between a `master` and a `slave`: it can
control `slave` nodes which are below it in hierarchy, but it also reports back to another
`inter` or `master`. There are a few control modes:
* single: commands are sent to a single node
* batch: all commands are sent to more than one node

Controlling nodes can be achieved by automating it on a `master` node, or by manual access via the web app
or API.



# Logs
Logs from the nodes will be retrieved by `Core` every five minutes (unless otherwise configured). The logs
are, by default, stored in `/etc/mond/logs/[node name]/logname`. `logname` is the usual name of the log,
appended by the time that it has been retrieved. `mond` will, one `slave` nodes, also monitor access to
system files like `/etc/shadow` and the sudoers file. 



# Miscellaneous
Python C-bindings for our API will be provided by us and will be used to run the web app and
web API. We do not want to force anyone into learning a certain language, so we will most likely
also provide C-bindings for other languages. Always feel free to contribute if you think a
necessary feature is missing!
