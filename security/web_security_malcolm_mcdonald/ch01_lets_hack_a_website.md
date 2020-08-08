# Chapter 1 - Let's hack a website

Hackers create exploits (code that takes advantage of a security flaw in software/websites etc)
  - White hats inform vendors/owners of the software/website to help fix
  - Black hats hoard exploits for their own use, or sell them on the dark web.

If exploit has been known for less than a day/not publicised at all, then it is a **zero-day**.

## How to hack a website
(The very simple version)

1. Install Kali onto a VM, run Metasploit
2. Run `load wmap` and then use the `wmap` plugin to scan for vulnerabilities on a specific domain
   or IP address
3. Use Metasploit to exploit the vulnerabilities found.
