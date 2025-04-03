---
title: Networking notes
description: Where is it
categories:
- General

feature_image: ""
---

``` mermaid!
mindmap
  root((Digital Communications Subsystems))
    Network interface
     NIC
      receive data from analog source
      compute simple things over data and place it into specific memory location defined by system driver
     DPU
      add more extensive compute capabilities to the network card
    System API
     Sockets API
     io_uring
     XDP sockets
    Network
     Devices
      hardware implementation of protocols
     Protocols
      Routing
       Act of choosing an output interface based on routing information
      Tunneling
       Adding or removing headers, while changing some fields
     Links
      Different kinds of connectors, logically they have a bandwidth and delay
     Flows 
      Identified by signature of traffic e.g. **n**tuple or some payload feature
```
