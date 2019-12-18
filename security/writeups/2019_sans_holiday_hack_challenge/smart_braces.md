From the documentation of the smart braces:
1. Set the default policies to DROP for the INPUT, FORWARD, and OUTPUT chains.
2. Create a rule to ACCEPT all connections that are ESTABLISHED,RELATED on the INPUT and the OUTPUT
   chains.
3. Create a rule to ACCEPT only remote source IP address 172.19.0.225 to access the local SSH
   server (on port 22).
4. Create a rule to ACCEPT any source IP to the local TCP services on ports 21 and 80.
5. Create a rule to ACCEPT all OUTPUT traffic with a destination TCP port of 80.
6. Create a rule applied to the INPUT chain to ACCEPT all traffic from the lo interface.

The man page for iptables can be found here: https://linux.die.net/man/8/iptables

So, going through the list:

## Set default policy to DROP
This is pretty simple - an example is even provided in the docs:

```
$ sudo iptables -P FORWARD DROP
$ sudo iptables -P INPUT DROP
$ sudo iptables -P OUTPUT DROP
```

## Create a rule to accept all connections that are ESTABLISHED, RELATED on INPUT and OUTPUT
Looking at the man page for iptables, we see that there are certain extensions we can activate,
using the `-m`/`--match` option, followed by the module name.

Using `-m conntrack`, we have an option called `--ctstate` that allows for a comma separated list of
"connection states" to track. These include the aforementioned `ESTABLISHED` and `RELATED`.

So we can accomplish this via:

```
$ sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
$ sudo iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

## Create a rule to ACCEPT only remote source IP address 172.19.0.225 on port 22
This is another one where a similar example is given in the Smart Braces docs:

After modifying the example we get:

```
$ sudo iptables -A INPUT -p tcp --dport 22 -s 172.19.0.225 -j ACCEPT
```

## Create a rule to accept traffic from any source IP on ports 21 and 80
Modifying above example (not specifying a source IP defaults to anywhere):

```
sudo iptables -A INPUT -p tcp --dport 21 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

Note: You can only specify one `--dport` at a time, so that's why we need two commands.

## Create a rule to ACCEPT all OUTPUT traffic with a destination TCP port of 80.

```
sudo iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT
```

## Create a rule applied to the INPUT chain to ACCEPT all traffic from the lo interface.
From the `iptables` man page, we see that the `-i` (`--in-interface`) option is used for this.

```
$ sudo iptables -A INPUT -i lo -j ACCEPT
```

-------------

For some reason, I've done the above steps multiple times but it wouldn't recognise it as
finished... It just works randomly. Maybe you have to execute `sudo iptables -L -v`? That's what I
did when I got it to work. Maybe it's a traffic thing.


