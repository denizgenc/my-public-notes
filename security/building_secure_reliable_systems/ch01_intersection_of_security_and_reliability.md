# Chapter 1 - The Intersection of Security and Reliability

## Basic concepts
To have a trustworthy system, reliability and security are essential.

Reliability and security share some common properties, but they have different design
considerations. These differences, and their interplay, can cause unexpected results.

### Case Study 2019-09-27 Google Wi-Fi incident
The following is an example of difficulties that can arise from the interaction of reliability and
security.

On 2019-09-27, a message went out saying that the Wi-Fi password on a fleet of buses, used at
Google's San Francisco location, had changed. Google had a password manager to share secrets, but it
was designed for smaller use cases, and having everyone that used the buses try to retrieve the new
password on the same day caused both the primary and secondary replicas of the database to fail.

An engineer based in NYC was assigned to the issue, and decided to restart the service to try to get
it to work again (they had never responded to a failure of the password manager). A restart of the
service required an HSM smart card. These cards were stored in safes in various different locations,
but not in NYC. One engineer in Australia could not remember the password for the safe, and used a
power drill to gain entry. Later, another engineer in California was able to remember the password
and retrieve the smart card.

After the engineers retrieved the smart cards, the password manager was restarted, and normal
service resumed.

----

In this case, the initial outage was a problem of reliability - load balancing/shedding was not good
enough. The recovery of the system was made more difficult by security controls.

## Design considerations for reliability and security
Risks are different for reliability compared to security:
- Risks for reliability are not malicious/motivated. Bad software update, etc. Something will go
  wrong _eventually_.
- Risks for security, however, are from adversaries. Something can go wrong _at any time_.
  - (Deniz: Don't we consider all the different types of vulnerabilities when doing security? e.g.
    natural disaster etc?)

In the case of reliability, you may decide to fail safe (or open): an electronic lock may unlock
itself in the case of a power outage, so people can safely leave the building. However, if your
threat model includes adversaries that would try to exploit (or create) power outages, you may
decide to fail secure: keep the electronic lock closed if there is no power.

There are compromises you may have to make between reliability and security:
- Redundancy:
  - Not necessarily just database replicas - physical key for electronic lock, fire escape =
    additional exits
  - Good for reliability - useful in case of failure of primary mode of operation of the system
  - Worse for security - increases the attack surface. Attacker only needs to compromise one of the
    redundant systems.
- Incident management
  - Reliability incidents benefit from more people responding, with different perspectives etc.
    Security incidents should minimise the amount of people responding, as this reduces the chance
    that an insider might leak the response effort to the attacker.
  - Lots of system logs can be useful from a reliability perspective, but may pose a security issue
    if an attacker gets hold of them.

## The CIA triad
