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
*Work in progress*
