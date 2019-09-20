NCSC/Cisco Cryptography update, 2019-09-20  
Notes taken by Deniz Genç. I have attempted to be accurate but there may be mistakes, omissions,
alterations for brevity or clarity, or my own opinions below.  
Do not assume that the notes below reflect the views of either Graham Bartlett or Cisco.

Graham Bartlett - Cisco Cryptography Update
-------------------------------------------

Agenda
------
1. IPsec VPN NCSC guidance (today)
2. IKEv2 post quantum preshared key IETF draft
3. IKEv2 post quantum key exchange IETF draft
4. SDWan
    - Why AES-256 is not always secure!
5. IPsec key exchange using a controller IETF draft
6. Network attacks against crypto

IPsec Guidance
-------------
- Don't just use the secure primitives that smart maths bods create, make sure your architecture etc
  is also secure
- There's also good guidance on PKI infrastructure from NCSC

Lots of links to look into:  
https://www.ncsc.gov.uk/guidance/using-ipsec-protect-data  
https://www.ncsc.gov.uk/guidance/acquiring-managing-and-disposing-network-devices  
etc

Problem today: IKEv2 security is based on Diffie-Hellman - it's vulnerable against quantum computers

Toy DH example: g = 2, p = 13  

- Client talks to VPN gateway. Generates DH private value 9.  
- Generates a public value via g^x mod p = 2^9 mod 13 = 512 mod 13 = 5  
- Client sends public value in the clear to VPN Gateway  
- VPN Gateway generates private value 11, g^x mod p etc (= 7)  
- Sends that value in the clear to client  
- Then both sides take others' public value and raise it to their private value mod 13 (either
  7 ^ 9 mod 13 or 5 ^ 11 mod 13)  
- Both calculations are guaranteed to produce the same value (8), which becomes the shared secret

A powerful enough quantum computer can take the public values and get the original private values.  
Therefore DH is broken/vulnerable.

IETF Standards timeline
-------------------
Proposed Standard -> Draft Standard -> Internet Standard  
Question: Are draft standards guaranteed to become a standard?

- No guarantee, kind of political. Friend has described it as "gladitorial".

There's a draft standard called "Postquantum Preshared Keys for IKEv2"
(link: https://tools.ietf.org/html/draft-ietf-ipsecme-qr-ikev2 )

How does it work?

- Client has database of the peers that it's going to talk to. Columns: Device name, PPK (the
  postquantum preshared key)
- Server has a similar database
- Same diffie hellman sharing of g ^ x mod p happens
- Then the client shares their "identity" to the server, server does same (identity should be
  "Device" in the database), along with a key that has the corresponding PPK value mixed in
- Even if someone breaks the Diffie Hellman step as above, they'll be able to see the identities but
  they won't know the PPKs, so they can't decrypt the connection.
     - Currently we don't know how to generate and share such a key. "Left as an exercise for the =
       reader".
     - Sakai/Kasahara (SAKKE) identity based encryption can apparently be used to distribute such
       keys ... but they're based on elliptic curves, which is quantum vulnerable

So basically this is kind of a stopgap before post quantum crypto (because of the difficulties with
getting the PPK to each node in the first place)

Protocol v0 for IKEv2 Postquantum key exchange
--------------------------
Client sends `SA_INIT SA KEi (DH) (QRKE1) (QRKE2) Ni` to server - QRKE1, QRKE2 are post quantum keys
(remember, they can be very large - 1kB to 1MB!)  
Server sends `SA_INIT SA KEr (DH) (QRKE1) (QRKE2) Nr` back  
IKE exchange is quantum resistant afterwards - keys were shared via DH, which can be broken, but the
IKE exchange itself afterwards won't be

Problem with version 0 - not very backwards compatible

Also, because the QRKE1/2 are very large, the frames have to be fragmented at the IP level - this
can cause issues in terms of opening up overlapping frame attack (since server has to allocate
memory for the next few frames), or the router might just not have enough memory in the first place 
for the next frames and will fall over.

The draft was then updated to deal with these attacks. The new process is the following:

- Client DHs to Server, telling them what they support
- Server responds by what they support out of those
- Then they send IKE frames of the keys - this means that the fragmentation happens at the IKE
  layer, so no chance of overlapping frame attack

Link to current draft: https://tools.ietf.org/id/draft-tjhai-ipsecme-hybrid-qske-ikev2

What do we need to deploy post-quantum security?
------------------
Timeline:

------------ Now ---------------

- First we need PQC standard exchange
- Vender interop
- Audit current crypto
- New hardware/software

------ 2022-2024 NIST algorithms -----

- Deploy quantum cryptography solutions - this won't be trivial

------ 2030: QC exists -------

Microsoft/Cisco took 6 years to implement IKEv2, Apple took 10, AWS took 14. So even if the NIST standards come out, it'll take a while for vendors to support it...

SD-WAN
---------------
What features Software Defined WAN (SD-WAN) have?

1. The ability to support multiple connection types, such as MPLS, frame relay, and higher cpacity
   LTE wireless comms
2. The ability to do dynamic path selection for load sharing and resiliency purposes
3. A simple interface that is easy to configure and manage
4. The ability to support VPNs, and third party services such as WAN optimisation controllers,
   firewalls and web gateways.

Every vendor implements SD-WAN/SD-WAN crypto in a different way

From the presentation SD-WAN New Hope
(https://github.com/sdnewhop/sdwannewhope/blob/master/slides/phdays-2019.pdf):

"SD-WAN product maturity:

- Complex products, open source based
- Problems with patch management
- Lots of management interfaces (and bugs)
- weak defaults
- Issues with patching/responsible disclosure
- ... in da cloud

So hack before you buy!"

IPsec Key Exchange using a Controller (Internet Draft Standard)
------------------
Instead of using IKE, do key exchange with controllers

1. 2 routers, 1 controller
2. 2 routers create a DH private number, then create a public number (g ^ x mod p etc)
3. send public to the controller, which distributes it to all the routers its connected to (will
   also send to routers 3 and 4 for example)
4. Shared secret generated
5. Traffic protected with shared secret

- Question: What problem does this solve?
    - If all nodes n in a network communicate with each other, need to create an order of O(n^2)
      tunnels.
    - This requires much more powerful hardware, which increases the cost of your network.
    - If you connect just one session per node to the controller, and perform key exchange from
      there, it reduces the complexity.

Network Crypto Attacks
--------------------
Diffie Hellman key space

- 1536-bit MODP group, not recommended any more
- 4096-bit MODP group -> attackers reduced it to 32 bits somehow

They used a 32 bit lookup table (~4 billion entries) to steal the private keys. Neat!
