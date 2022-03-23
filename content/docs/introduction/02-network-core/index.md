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

{{< alert context="success" >}}

Let's unpack that last statement just a little bit because we actually heard a lot of important words.

- forwarding
- a path from source to destination
- a source and a destination

{{< /alert >}}

### Forwarding & Routing

There are two key functions performed inside the network core **forwarding**, sometimes also known as **switching**, and **routing**. Let's take a look inside a router so we can see what those functions are. **Forwarding** is a local action, it's about moving arriving packets from router's input link to appropriate router output link. **Forwarding** is controled by the forwarding table inside each and every of the millions of routes in the Internet. When a packet arrives a router will look inside the packet for a destination address and then look up the destination address in its forwarding table and then transmit that incoming packet on the output link that leads to that destination. Well, that's pretty simple conceptually, lookup and forwarding. But you might be wondering how are the contents of that forwarding table created int the first place and that's where we encounter the second key function of the network core -- **routing**. **Routing** is a global action of determining the source to destination path taken by packets. As we'll see routing algorithms compute these paths and compute the local per router forwarding tables needed to realize this end-to-end forwarding path.

{{< details "A good analogy for understanding the difference between forwarding and routing" >}}
{{< figure src="routing.jpg" caption="routing" width="80%" class="tac" >}}
Think about taking a trip in a car. I decide to take this upper route here rather than this lower route here and that's the routing decision that's made the path that's taken from the source to destination. Now when I get to an interchange, I'm coming into the city on one of the input roads and I need to be forwarded out of the city on an output road in Cleveland here and really at all intersections along the way that switching from an input road to an output road is the local forwarding function with the global routing function(forwarding table) determining which the output roads I'm actually forwarded onto.
{{< figure src="forwarding.jpg" caption="forwarding" width="80%" class="tac" >}}
{{< /details >}}

### Store and Forward

Let's next focus on the transmission of packet bits at a router in the network core.

{{< figure src="store-and-forward.jpg" caption="store and forward" width="80%" class="tac" >}}

- **packet transmission delay:** takes L/R seconds to transmit (push out) L-bit packet into link at R bps
- **store and forward:** entire packet must arrive at router before it can be transmitted on next link
  {{< alert context="warning" >}}
  Transmitted bits will be received and gathered up at the receiving end of a link. Until the full packet has been received, it can then be forwarded on to the next hop and so on. This is what's know as the store and forward operation of a packet switch network.
  {{< /alert >}}

### Ququing and Loss

Now let's take a closer look at what happens as packet arrived to a router for forwarding.

{{< figure src="router-forwarding-quque.jpg" caption="router forwarding quque(A->C, B->E)" width="80%" class="tac" >}}

Let's take a close look at the input link rates. The transmission rate R of the link from A to the first hop router is 100 Mb/s as is the second link from B to the first hop router. But the transmission rate of the link from the first hop router to the second hop router is only 1.5 Mb/s.

{{< alert context="warning" >}}
It's almost 100 times slower and this isn't far from the case in a home network where the home network router may have attached ethernets in the home running at a Gb/s or wi-fi at 54 Mb/s but the access link to the cable heading is much slower perhaps only 5 or 10 Mb/s.
{{< /alert >}}

So here's the question what happens as packets arrive to this first hop router. Well the router in this example can only transmit at 1.5 megabits per second and certainly packets can arrive a lot faster than 1.5 megabits per second if A and B are both transmitting a lot of packets at the same time. If too many packets arrive at too fast a rate, then a queue of packets will form in that first hop router as shown in the figure. Queuing happens whenever work arrives faster than some service facility can actually serve that work.

{{< alert context="warning" >}}
than 后面是一个从句  
翻译成每当工作（packets）到达的速度快于某些服务设施实际为该工作提供服务的速度时，就会发生排队
{{< /alert >}}

**Packet queuing and loss:** if arrival rate to link exceeds transmission rate of link for some period of time:

- packets will queue, waiting to be transmitted on output link
- packets can be dropped(lost) if memory(buffer) in router fills up

We'll see the fact that packets can be delayed and or lost is going to be a major source of headaches for a lot of network protocols. Packet queuing delays and packet loss can happen because the network doesn't always control senders carefully enough to ensure that large queuing delays and packet loss doesn't happen.

## Circuit Switching

Well, packet switching is not the only way to build a network. And indeed long before the internet was around and long before packet switching, telephone networks employed a different form of technology known as circuit switching. Let's take a look at circuit switching.

In circuit switching, there's a notion of a call rather than a notion of packets that flow from source to destination. Before a call starts, all of the resources within the network that are going to be needed for that call are allocated to that call from source to destination. So once the call begins the call will have reserved enough transmission capacity for itself to ensure that queuing will never occur. There's no delay other than propagation delay and no loss of data within the network because link capacity has been reserved for the exclusive use of this call.

{{< figure src="circuit-switching.jpg" caption="Alternative to packet switching: circuit switching" width="30%" class="tac" >}}

In this diagram here each link has four circuits. The call from the top left to the bottom right is allocated the second circuit on the top link. And the first circuit on the right link the circuits are dedicated resources. They're not shared with any other users. It's really like there's a wire from source to destination.

So all this sounds pretty good, no delays, no loss, doesn't get any better than that, until you start to think about the fact that since resources are reserved for the exclusive use of a call but the circuits can go idle if there's no data to send on that call and there's the rub if the bandwidth isn't used by the call. It's lost. No one else and no other calls can use it and so a circuit switch network can be inefficient.
