---
title: "Network Core"
description: "We’re going to see what happens inside the network core and introduce a number of important topics."
lead: "We’re going to see what happens inside the network core and introduce a number of important topics."
date: 2020-10-06T08:49:31+00:00
lastmod: 2020-10-06T08:49:31+00:00
draft: false
images: []
menu:
  docs:
    parent: "introduction"
weight: 630
toc: true
---

- forwarding and routing
- packet quueing delays and packet loss
- circuit switching(alternative to packet switching)
- the structure of the Internet
  - we will see what we mean when we say the Internet is a network of networks

## Packet switching

The network core consists mesh of routers that are interconnected by a set of communication links and the Internet's core operation is based on a principle known as packet switching which is really relatively simple.

The idea is the following, what happens is that the end hosts take application level messages, divide those messages into chunks of data, put those chunks of data inside packets and send those packets into the Internet. Now those packets are then forwarded along a path from a source node to a destination node.

{{< alert icon="👉" >}}

Let's unpack that last statement just a little bit because we actually heard a lot of important words.

- forwarding
- a path from source to destination
- a source and a destination

{{< /alert >}}

### Forwarding & Routing

There are two key functions performed inside the network core **forwarding**, sometimes also known as **switching**, and **routing**. Let's take a look inside a router so we can see what those functions are. Forwarding is a local action, it's about moving arriving packets from router's input link to appropriate router output link. **Forwarding** is controled by the forwarding table inside each and every of the millions of routes in the Internet. When a packet arrives a router will look inside the packet for a destination address and then look up the destination address in its forwarding table and then transmit that incoming packet on the output link that leads to that destination. Well, that's pretty simple conceptually, lookup and forwarding. But you might be wondering how are the contents of that forwarding table created int the first place and that's where we encounter the second key function of the network core -- **routing**. **Routing** is a global action of determining the source to destination path taken by packets. As we'll see routing algorithms compute these paths and compute the local per router forwarding tables needed to realize this end-to-end forwarding path.

### An Analogy

A good analogy for understanding the difference between forwarding and routing is to think about taking a trip in a car. I decide to take this upper route here rather than this lower route here and that's the routing decision that's made the path that's taken from the source to destination. Now when I get to an interchange, I'm coming into the city on one of the input roads and I need to be forwarded out of the city on an output road in Cleveland here and really at all intersections along the way that switching from an input road to an output road is the local forwarding function with the global routing function(forwarding table) determining which the output roads I'm actually forwarded onto.
