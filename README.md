# Custom-Transport-Protocol

__NOTE:__ I am not allowed to publicly post this. Please email rkwong12@gmail.com to see the source code.

Designed a UDP-based multiplicative increase transport protocol to send datagrams.

Features a receiver and a sender program that communicate to send information from the command line.

Both the receiver and sender will also print out to STDERR any errors and information received/sent for
debugging and keeping track of data.

Another part of the project was to maximize performance while keeping the same reliability.
In doing so, a variant of the slow start congestion control and sliding window were used.
The sender uses a sliding window with multiplicative increase of packets sent and upon hitting a
timeout, resets the packets sent back to 1.
Because the UDP protocol was used and expanded into a TCP-like two-direction communication, the timeout
period for the sockets were reduced largely to increase performance for dropped or missing packets.

The sender and receiver bases most of its communication about errors through timeouts. Timing out for the sender
will have it send all the packets sent in the last round of the sliding window.
The receiver will ACK or acknowledge all packets received. Upon receiving a timeout, will resend the ACKs.

On exiting, both will communicate with each other to close down. The receiver will close immediately after
sending confirmation ACKs and the sender will close after receiving those ACKs.
In the case packets are dropped during the shutdown, the sender will repeatedly send packets to shut the receiver down
or until a certain amount of timeouts happen.

Command line syntax:
Receiver:
./3700send <recv_host>:<recv_port>

Included is a net simulator that will simulate certain conditions to test reliability of the protocol.
Conditions include:

    bandwidth: This sets the bandwidth of the link in Mbit per second. If not specified, this is 1 Mb/s.
    latency: This sets the latency of the link in ms. If not specified, this value is 10 ms.
    delay: This sets the percent of packets the emulator should delay. If not specified, this is 0.
    drop: This sets the percent of packets the emulator should drop. If not specified, this is 0.
    reorder: This sets the percent of packets the emulator should reorder. If not specified, this is 0.
    duplicate: This sets the percent of packets the emulator should duplicate. If not specified, this is 0.

Command line syntax:
netsim [--bandwidth <bw-in-mbps>] 
       [--latency <latency-in-ms>] [--delay <percent>] 
       [--drop <percent>] 
       [--reorder <percent>] [--duplicate <percent>]
  
Additionally there is a helper script included to run the protocol on specified data sizes.
Command line syntax:
./run [--size (small|medium|large|huge)] [--printlog] [--timeout <seconds>]
  
The helper script and net simulator were starter code, and unedited by me.
