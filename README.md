# HW3: ICMP Ping and Traceroute

_Attribution: this assignment is based on ICMP Pinger Lab and ICMP Traceroute Lab from Computer Networking: a Top-Down Approach by Jim Kurose and Keith Ross. It was modified for use in CSC249: Networks at Smith College by R. Jordan Crouser in Fall 2022._

In this assignment, you will gain a better understanding of Internet Control Message Protocol (ICMP) by implementing your own **`ping`** and **`traceroute`** applications using ICMP request and reply messages.

As we saw in A1, `ping` is a computer network application used to test whether a particular host is reachable across an IP network. It is also used to self-test the network interface card of the computer or as a latency test. It works by sending ICMP `echo reply` packets to the target host and listening for ICMP `echo reply` packets in response (the return `echo reply` is sometimes called a _pong_). As written, `ping` measures the round-trip time, records packet loss, and prints a statistical summary of the echo reply packets received (min, max, and the mean of the round-trip times and in some versions the standard deviation of the mean).

Your task is to develop your own `ping` application in Python. Your application will use ICMP but, to keep it simple, will not exactly follow the official specification in [RFC 1739](https://datatracker.ietf.org/doc/html/rfc1739). 

_Note: you will only need to write the client side of these program, as the functionality needed on the server side is built into almost all operating systems._

## Part 1: `ping`
Your `ping` application should send `ping` requests to a specified host separated by approximately one second. Each message will contain a payload of data that includes a timestamp. After sending each packet, the application should wait up to one second to receive a reply. If one second goes by without a reply from the server, then the client should assume that either the ping packet or the pong packet was lost in the network (or that the server is down).

### Support Code
The file `ICMPpinger.py` contains starter code for the client-side behavior of your `ping` program. Your task is to fill in code in the area marked with `#Fill in start` and `#Fill in end` in order to get your program to behave as described.

### Details
1.	In the `receiveOnePing(...)` method, you need to receive the structure `ICMP_ECHO_REPLY` and fetch the information you need, such as checksum, sequence number, time to live (TTL), etc. Take a look at the `sendOnePing(...)` method and make sure you understand what it is doing before trying to complete the `receiveOnePing(...)` method.
2.	You do not need to be concerned about the `checksum(...)` method, as it is already given in the code.
3.	This exercise requires the use of raw sockets. In some operating systems, you may need **administrator/root privileges** to be able to run your pinger program (i.e., you'll run the program with the command `sudo python ICMPpinger.py`).

### Testing the Pinger
First, test your client by sending packets to localhost, that is, 127.0.0.1. Then, play around to see how your Pinger application communicates across the network by pinging servers on different continents - feel free to use the ones you found for A1!

## Part 2: `traceroute`
In this section, we'll expand on what you just wrote in order to implement a traceroute application using ICMP request and reply messages. You are strongly encouraged to first do the `ping` section before attempting the `traceroute` section, as it is done with the same approach. The checksum and header making are not reiterated here; refer to the previous `ping` section for that purpose. The naming of most of the variables and socket is also the same.

`traceroute` is a computer networking diagnostic tool which allows a user to trace the route from a host running the `traceroute` program to any other host in the world. As with `ping`, `traceroute` is implemented with ICMP messages. It works by sending ICMP `echo` (ICMP type ‘8’) messages to the same destination with increasing value of the time-to-live (TTL) field. The routers along the traceroute path return ICMP `Time Exceeded` (ICMP type ‘11’ ) when the TTL field become zero. The final destination sends an ICMP `reply` (ICMP type ’0’ ) messages on receiving the ICMP `echo` request. The IP addresses of the routers which send replies can be extracted from the received packets. The round-trip time between the sending host and a router is determined by setting a timer at the sending host.

Your task is to develop your own `traceroute` application in python using ICMP. Your application will use ICMP but again, in order to keep it simple, will not exactly follow the official specification in RFC 1739.

### Support Code
The file `ICMPtraceroute.py` contains starter code for the client-side behavior of your `traceroute` program. Your task is to fill in code in the areas marked with `#Fill in start` and `#Fill in end` in order to get your program to behave as described.

### Details
1. As before, this exercise also requires the use of raw sockets. In some operating systems, you may need administrator/root privileges to be able to run your `traceroute` program.
2. This will not work for websites that block ICMP traffic.
3. You will have to turn your firewall or antivirus software off to allow the messages to be sent and received.

### Optional Feature
Currently the application only prints out a list of ip addresses of all the routers along the path from source to the destination. Try using the `gethostbyname(...)` method to print out the names of each intermediate route along the route.
