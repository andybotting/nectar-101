---
title: Security Groups
permalink: /security-groups
icon: lock
order: 03
---

Now that our Instance is `Active`, the next step would be to connect to it to start doing something useful.

If we try and ping our Instance, we'll find that we get no reponse back.

```console
$ ping 130.56.248.49
PING 130.56.248.49 (130.56.248.49) 56(84) bytes of data.
^C
--- 130.56.248.49 ping statistics ---
5 packets transmitted, 0 received, 100% packet loss, time 48ms
```

This is because all incoming network traffic is blocked by default. This is where `Security Groups` come in.

## Creating a new security group

Security Groups can be managed from the Research Cloud Dashboard, and the rules that live inside them.

From the left navigation menu, go to [Project > Network > Security Groups](https://dashboard.rc.nectar.org.au/project/security_groups/). By default, a single security group is provided for you, called `default`. This group allows all egress (outgoing) traffic from your Instances to the Internet.

For ingress (incoming) traffic, we will create a new `Security Group`. Click the `Create Security Group` button, and give it a name.

Once created, click the `Manage Rules` button to the right of the new Security Group. You'll notice that the group already contains two rules; these allow all IPv4 and IPv6 egress traffic, so your Instance can communicate out to the Internet.

For our example, we're going to allow ingress (inbound) traffic to our new Instance:
* `All ICMP` for ping
* `HTTP` for a webserver (TCP port 80)
* `SSH` for remote console (TCP port 22)

For each of these services, click the `Add Rule` button on the `Manage Security Group Rules` page of our new Security Group and add a rule for each of these services. We will accept the defaults given and create an `Ingress` rule, using `0.0.0.0/0` for our CIDR. This address basically tranlates to `any IPv4 address`. This will mean that any host on the Internet will unblocked.

For the SSH rule, we'll limit access for security reasons. If we go to <https://www.whatismyip.com/>, it will give is our current IPv4 address. Use this address for our CIDR value so SSH access is only available from the current network.

Once the three security group rules have been added, we'll apply this group to our new Instance.

## Applying the security group rules

Go back to the [Project > Compute -> Instances](https://dashboard.rc.nectar.org.au/project/instances/) page, click the drop down arrow next to our new Instance and choose `Edit Security Groups` menu item. You should see your new Security Group listed in the left `All Security Groups` column. Click the `+` button to move it to the right `Instance Security Groups` column and click `Save`. The Security Group will now be applied to our Instance.

If we revisit our `ping` test from earlier, we'll find this time it's successful.

```console
$ ping 130.56.248.49
PING 130.56.248.49 (130.56.248.49) 56(84) bytes of data.
64 bytes from 130.56.248.49: icmp_seq=1 ttl=52 time=8.21 ms
64 bytes from 130.56.248.49: icmp_seq=2 ttl=52 time=8.03 ms
64 bytes from 130.56.248.49: icmp_seq=3 ttl=52 time=8.07 ms
64 bytes from 130.56.248.49: icmp_seq=4 ttl=52 time=8.20 ms
^C
--- 130.56.248.49 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 6ms
rtt min/avg/max/mdev = 8.028/8.126/8.210/0.120 ms
```

Let's try using SSH now to do something useful with our Instance.
