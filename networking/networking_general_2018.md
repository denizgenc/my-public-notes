Talk on 2018-11-08
-----
- Last time talked about networks and structures being layered
- Touched on idea that network info was structured in packets/frames (ethernet
  level)

We're going to look at a network through a packet sniffer (Wireshark)
  - Just going to be able to see stuff that goes over our own network
    interface
  - Could set it up to see stuff that goes across our interface, which can be
    used to see other people's traffic.

TCP and UDP ports are SEPARATE THINGS
  - for example: SSH is *TCP* port 22. What is UDP port 22? Who knows

UDP is good for time sensitive
  - Doesn't guarantee that packets will come in order
  - Doesn't guarantee that packets will even arrive
  - Doesn't really have a concept of connections - it's just chucking packets
    at the other host. Doesn't know if they are received or whatever.

TCP
  - Good for everything that UDP doesn't do
  - However it's sluggish, since it has to bundle up multiple datagrams, and
    you can't control the timing of the packet being sent, unlike UDP.

Ports
  - Two different types of ports: destination ports and source ports.
  - Open two web browsers and point them at the same address. Both send TCP
    packets with a destination port of 80. There will be 2 replies from the
    web server - how will your computer know which one goes to which browser?
    Source ports! **This is why source ports are generally randomised.**

Talk on 2018-11-15
-----
- HTTP
  - We make a request from our internal IP address (192.168.?.?) to an
    external one (52.?.?.?- alwayshttp.com)
      - This establishes a TCP connection (not UDP)
  - How does alwayshttp.com know where to send the website data to?
      - Obviously can't send it to 192.168.x.y, since that's an internal IP.

  - Our router replaces the IP address in the HTTP request from our private
    IP to the public IP of the router, so alwayshttp.com can reply back!

  - However this creates a new problem - alwayshttp.com will be sending its
    response to the router, not our computer.
      - The router needs a way to remember this TCP connection.
          - You can't just remember the requested site, then forward stuff
            from that site to the PC that requested it - what if all the PCs
            on the network were requesting alwayshttp.com? Router would get
            confused

  - To solve it, use NAT.

- Network Address Translation
  - Explanation from Wikipedia:

    "As traffic passes from the local network to the Internet, the source
    address in each packet is translated on the fly from a private address
    to the public address. The router tracks basic data about each active
    connection (particularly the destination address and port). When a reply
    returns to the router, it uses the connection tracking data it stored
    during the outbound phase to determine the private address on the
    internal network to which to forward the reply."

  - Also:

    "The vast bulk of Internet traffic uses Transmission Control Protocol
    (TCP) or User Datagram Protocol (UDP). For these protocols the port
    numbers are changed so that the combination of IP address and port
    information on the returned packet can be unambiguously mapped to the 
    corresponding private network destination. RFC 2663 uses the term
    network address and port translation (NAPT) for this type of NAT. Other
    names include port address translation (PAT), IP masquerading, NAT
    overload and many-to-one NAT. This is the most common type of NAT and
    has become synonymous with the term "NAT" in common usage."

- UDP
  - The above method doesn't work for UDP because it doesn't have the concept
    of a "connection" - it just flings packets at the destination and doesn't
    give a shit.
  - So instead the source PC opens a UDP port
  - The router does the substition of the source IP, changing it from the
    private address used by the PC to the public one by the router, and
    remembers the port for a certain amount of time - time to live (TTL). 
  - Once the TTL expires, it forgets about the port, so if a UDP reply doesn't
    come back in time lmao @ u fam

- All of the above is OK if you need to talk to a server with a publically
  available IP address that you can find via DNS, etc. But what if you need to
  establish a connection to someone else who's behind a NAT? Say, if you're
  playing a video game online?
  - One thing you can do is set up port forwarding (also called port mapping):
    tell your router that "if you get packets from outside matching these 
    TCP/UDP ports, forward them to this private IP address."
      - Downside 1: Difficult to do for beginners (not only the port
        forwarding part, but you also have to disable DHCP for the device you
        want to forward to)
      - Downside 2: Essentially creates a hole in your device's defenses,
        allowing anyone to poke your device if they use those ports.

  - Another solution - for UDP only - is methods such as:
      - STUN (Session Traversal Utilities for NAT)
      - TURN (Traversal Using Relay NAT) -> improvement on STUN
      - ICE (Interactive Connectivity Establishment) -> synthesis of 2 above

        ^ These are all hole punching methods/protocols

      These basically work by using a rendezvous (think matchmaking) server as
      a middleman. You open a UDP port for a bit, and then tell a STUN server 
      "Hey, I'm open to UDP connections on this port." The STUN server then 
      finds someone else to connect to you, and exchanges public IPs and open 
      UDP ports between the two clients. Then you start chatting away.

Talk on 2018-11-22
-----
Layer 2 addresses == MAC addresses
  - Flat addressing scheme (no authority for who gets what address)

Layer 3 addresses == IP addresses (one possible form)
  - Hierarchical addressing scheme (Providers -> ISPs -> whatever -> you)

You can randomise your MAC address and stuff still works. So why bother with IP
addresses?

Ye Olden Ethernet days:
- Hubs
  - Just amplified and broadcasted data to everything connected to it - no
    routing
  - Basically just people shouting in a room, and your computer would have to
    listen in and "hear" when its MAC address was named
  - Easy to exploit, obviously
- Switches
  - Actually performs routing
  - However, need any configuration - you don't need to tell it which PCs have
    which MAC address.
      - It does this through a process ccalled learning.
      - https://en.wikipedia.org/wiki/Bridging_(networking)

