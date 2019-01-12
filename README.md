# About Show Composer
This Repo contains Show Composer's concept and structure

## What is "Show Composer"?
Show Composer is currently an idea to make a new kind of event mangement and show running software.

We want to make an modular but well integrated, open software to run your show!
Our first planned steps are the implementation of the basics, and the implementation of a converter-kind subsystem for managing different controls and signals.

## What do we want?
Show Composer want's to be a tool for you, to easily compose your show - regardless in which scale - and be creative, without bothering to much about technical details. 
We hope that Show Composer becomes an modular and rock solid tool to manage and run all your lightning, video, playback, time management, lineup and the other stuff you need.

The idea behind making Show Composer open source is that everyone can use it - and hopefully help us to make it better.
Everyone means everyone. The idea is a tool, which can control a few LED's in your basement up to a few hundreds of fixtures in an arena. 

**But we need you to help!**

## How can I help?
There are many ways: If you know JavaScript, you can look into our implementation and improve it, fix bugs or implement new features. The Programm architecture needs to be improved continously. We need a good documentation, or you can write tutorials and help other users. 

Just come up with your idea!

# Modules
Show Composer is split into modules, which all handle "their thing". The following modules are planned:
## Visuals
Bundles the control, effects etc. of all Visual Elements: Videos, "classical" Lighning and everything with pixels
## Communicator
Handles all inputs/outputs, patches them and map them to actions.
Examples:
- ArtNet
- MIDI
- OSC
- HTTP
- MQTT
## Manager
Has a lineup and all the "paperwork" needed to run a show.
## System
Manages and monitors ShowComposer itself: Discovering and provisioning of new nodes, resource overview, data storage, project export- and import, redundancy and failover

# Technical Overview
In this part the main ideas of Show Composer are explained. They are: Which kind of participants in a network you can have, and (basically) how they communicate/data is stored (SC-data).
## Nodes
A (virtual) device taking part in a Show Composer-Session is refered as a "node". A node can have multiple Modules on it.
At this time, the following Node Types are planned:
- **Master or Server** core of every Show Composer installation, at least one needed. Does all the computation and logical stuff. Can run on a headless system (Server). If multiple masters are in a network, one is the primary master, and the others are standby masters, taking computational load from the master and taking over when the primary is gone.
- **User Interface** Means everything displaying information and taking user input which is connected over SC-data. Webinterface is not a Node, but communicates over HTTP(S) and Websockets to the master.
- **Satellite** A satellite is a small application (or hardware), which runs headless and is communicating over SC-data. A Satellite does I/O for stuff which can't have I/O at the master: MIDI, DMX (without ArtNet), Audio, or simple Buttons. The idea is to configure satellite fully trough the User Interface, so you just plug in power and everything runs.
- **Cloud** A cloud node is not "on site", but at an server over the internet. The connection to a cloud node is considerd unstable and insecure, so it's not used for critical live stuff and the connection is encrypted by default. All the permanent data is backed up into the cloud regulary. Another usecase is remote editing/preprogramming and merging of changes with the local, "onsite" system.
## Data
Show Composer will use an object-based system for storing all the data. One example:
`visuals.fixtures.id[0].name="testfixture"`

All the data is handled by a central component, the router, which ensures every change and state will be on the right place the right time. It's possible to subscribe to one hierarchy, for example get all changes from `visuals.fixtues.id[0].params`.

### Types
The following types are supported:
- STATIC: Is part of the project definition, saved and eventually exported
- LIVE: has a value, but it's not persistent (e.g. step of the current cuelist, current channel value)
- TICK: only fires in a moment, describes an event (e.g. button press)
- LINK: this node is an alias for another node (e.g. `visuals.fixtures.group['frontlight'].fixture[0]`  is an alias for `visuals.fixtures.id[10]`)

### Connection types

There are two different types of connections: **Backend** and **Client**.

Backend-connections are used to connect all cores in a installation to each other, while Client-connections are used to connect the modules to the core.

All core-instances should be connected in a full mesh to each other (every core has a connection to each other core), while a client is only connected to a single core (usually the local one).

If a core-instance receives a change on a client-connection, this change should be forwarded to all backend-connections. If a change is received via an backend-connection, it should be forwarded only to the client-connections to avoid loops.

### Methods

Inside the TCP-based protocol the following methods are used (C: only by client, S: only by server, C/S: both):

C: `<REQ_ID> INIT <PROT_VERS>` inits the connection with the server, sends its Protocol Version
C: `<REQ_ID> INIT_REUSE <PROT_VERS> <CONN_ID>` inits the connection with the server while reusing an old connection ID, sends its Protocol Version. Mainly used after a connection is broken.
S: `<REQ_ID> INIT_ACK <PROT_VERS> <CONN_ID>` Acknowledge of the connection, sends server protocol version and connection id, connection is now up.

Note: REQ_ID is an integer used to identify the response.

### Connection states
Each connection has its own connection id. All states of this connection are stored inside `system.connections[<CONN_ID>]` and are handled/exported by the datalib.

The following parameters should be existent:

`state`:`UP`,`CLOSED` or `BROKEN`

`latency`: Latency in µs

`server`: Reference to server host

`client`: Reference to client host

`time_established`: When the connection was established the last time, Unixtime or undefinded

`time_closed`: Time the connection was closed normally, Unixtime or undefined

`time_broken`: Time the connection broke, Unixtime or undefined





