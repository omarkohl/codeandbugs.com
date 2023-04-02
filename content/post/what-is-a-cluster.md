---
title: "What is a Cluster?"
date: 2023-04-02T11:29:21+02:00
draft: true
---

Recently someone told me that they never really understood what a computer
cluster was, so I tried to come up with a simple analogy that they then
declared excellent.

This is Arvid.

Arvid gives financial advice (e.g. what stocks to buy) to people who ask him.

This works quite well for Arvid's customers, except for a few things:

* When Arvid is sick he does not answer the phone.
* If Arvid were ever to die all the things he keeps in his head would die with
  him and his clients would suffer with it.
* As a single person Arvid has limited memory to learn facts about the world
  that help with his advice.

Arvid is the IT analogy of a server. A server is a big, powerful computer that
has no screen, mouse or keyboard connected to it, but is instead connected to a
computer network. The way to reach the server is via its IP address (the
equivalent of Arvid's phone number).

Such a server works very well, except for a few things:

* If the server has some hardware defect it no longer replies to network
  requests.
* If the server gets fried by an electric shock all the data it stores would be
  lost.
* The amount of disk space of the server is limited, so it can only store so
  much of you films and photos.

How could be improve the experience for Arvid's customers? Arvid could found a
company "GetRich Inc." and employ some other advisors to work with him.

These are Berta and Cesar.

The simples change Arvid could make is to hand out a list of all phone number
to all customers so if they try to call Arvid and he does not pick up they can
try Berta next, then Cesar etc. Also, Arvid, Berta and Cesar can talk to each
other when a customer question comes in, making use of their different
knowledge to better answer the question. Finally, the three could purposefully
decide to accumulate some of the same knowledge so that even if one of them
becomes permanently incapacitated no knowledge would be lost.

From the customer's point of view they get:

* Better availability (it is much more unlikely that Arvid, Berta and Cesar all
  get sick at the same time).
* Better knowledge protection because the three advisors share their facts with
  each other.
* Access to more knowledge because it is the aggregate of the facts that the
  three advisors possess.

The IT analogy for "GetRich Inc." is a cluster of servers.

* A client can reach any IP address of a server.
* The servers ensure to send the data to each other in order to store it
  redundantely (e.g. two copies of each file) and to distribute it among
  themselves.
* The total disk space is the sum of all the server (discounting some space
  required for redundant data storage) so there is more storage available.

What is not so great is that the customer needs to know the phone number of
every employee of GetRich Inc. and they need to call every employee in turn
until the encounter one that answers the phone.

How could we improve upon this?


## Virtual IP

GetRich Inc. could get a customer hotline phone number. They would configure it
so that the hotline is always forwarded to a specific employee that is not
sick. This employee could for example change weekly (unless they get sick, in
which case the hotline number would be forwarded to another employee).

From the customer's point of view this is great because they get all the above
advantages of contacting a company instead of a person and in addition they
only need to remember and use a single phone number.

The IT analogy is for this customer hotline phone number is a virtual IP or
cluster IP. This IP is always assigned to one of the servers, but it can switch
between them. A client never knows which server has the IP, nor do they care
because by using the IP can they interact with the cluster the same way as with
the specific IP address of a server, but they don't have to remember all the
individual IPs nor do they need to try different IPs until they reach a working
server.

Using a customer hotline phone numbers that is forwarded to a particular person
(virtual IP) works very well, but has one downside. There is always only one
person (server) getting all the calls, meaning that they can get overwhelmed
even though the other advisors (servers) are mostly bored.


## Load Balancer

One way of improving upon this is a load balancer. Yvan knows all the
individual phone numbers of the advisors working at "GetRich Inc." so whenever
a customer needs some advice he just calls Yvan. Yvan then routes the call to
one of the GetRich advisors. Yvan also makes very short calls to all the
GetRich advisors every 10 minutes to ensure that they are not sick and he keeps
a list of who is currently available. This way he can ensure that customer
calls always get routed to someone who is ready.

The advantage of this setup is that Yvan can equally distribute all incoming
calls among all advisors who are available, ensuring nobody gets overwhelmed.

The disadvantage of this setup is that Yvan himself becomes a
single-point-of-failure. What happens if Yvan gets sick? Also, by routing all
calls through Yvan you are introducing a little bit of slowness (latency) into
the phone calls. Another disadvantage is the necessity for Yvan himself to
exist. He is an employee who needs to get paid, whereas the (virtual IP) call
forwarding is basically free.

There is no way of strictly saying whether a load balancer is better or worse
than a virtual IP. It is a tradeoff.


## DNS Round Robin

Yet another way of solving the issue of a single person receiving all the calls
is DNS Round Robin.

This is Xavier. He keeps a list of names and phone numbers. You can call him
and ask him, what is the number of Erica? He will then give you her phone
number. This has the advantage that you don't need to remember phone numbers
but names, which is easier, and also that if Erica wants to change her number
she only has to tell Xavier to do so and she is confident everyone will get the
message.

The IT equivalent of this is a DNS server, which keeps a list of domain names
(e.g. google.com) and their associated IP addresses.

Xavier can also return a phone number when you ask him for "GetRich Inc."'s
number, but he could return a different number every time he is asked! This
way, no customer needs to memorize any phone number whatsoever but all calls
will be distributed among the advisors.

Round Robin is just a fancy way of saying "in this list choose a different one
every time and in the end go back to the beginning of the list".

The disadvantages of this setup is that Xavier knows nothing about the
availability of the advisors so he might return a phone number of someone is
sick. Also, again he is an additional employee who needs a salary and he needs
some internal knowledge about "GetRich Inc.".


## Final words

A cluster is a way of interacting with a group of servers without needing to
care about their individual availability or how they distribute data among
themselves.  There are several ways of improving upon this, in particular
virtual IPs, load balancers and DNS round robin, each with advantages and
disadvantages.

All of this is a simplification and if you go more into detail the analogies
break down. Also note that it is possible to combine multiple approaches into
one. In any case, it is useful to start building a simple mental model and then
if needed attach exceptions and extensions to it, making it more complex.
