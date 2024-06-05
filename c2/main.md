## QUESTIONS:

The following text contains questions and answers. Each answer needs to provide a satisfactory response to the corresponding question. Verify the validity of each answer.

### 2.1

    1Q: List five nonproprietary Internet applications and the application layer protocols that they use. 
    1A: Web - HTTP 1.1, Email - SMTP, IMAP and POP3, File transfer - FTP, SFTP (Secure File Transfer), Streaming multimedia - HTTP / DASH, Remote terminal access - Telnet, SSH

    2Q: What is the difference between network architecture and application architeture?
    2A: Network architecture refers to the different layers (Application, Transport, Network, Link, Physical) that enable various methods of network communications.
    Application architecture is designed by application developer and dictates how the application is structured on various end systems.

    3Q: For a communication session between a pair of processes, which process is the client and which the server?
    3A: The process that initiates the communication is the client and the process responding is the server.

    4Q: Why are the terms client and server still used in p2p applications?
    4A: Because each peer is acting both as a client and as a server, potentially at the same time.

    5Q: What info is used by a process running on one host to identify a process running on another host?
    5A: IP address and port number.

    6Q: What is the role of HTTP in a network application? What other components are needed to complete a Web app?
    6A: It provides semantics enabling multiple autonomous developers to develop client and server applications that can communicate with each other.
    A web server software, client side software (also potentially a database server).

    7Q: Referring to Figure 2.4, we see that none of the applications listed requires both no data loss and timing.
    Can you conceive of an application that requires no data loss and that is also highly time-sensitive?
    7A: Trading bot / an exchange.

    8Q: List the four broad classes of services that a transport protocol can provide. For each of the service classes,
    indicate if either UDP or TCP (or both) provides such a service.
    8A: - Reliable data transfer: Provided by TCP. TCP ensures that data is delivered accurately and in order, retransmitting lost packets if necessary.
        - Throughput: TCP can provide guaranteed throughput to some extent, as it manages congestion control and flow control to optimize data flow.
        UDP does not provide guaranteed throughput, but it can achieve higher throughput in applications where occasional data loss is acceptable because it has less overhead.
        - Timing: Neither TCP nor UDP inherently guarantees timing (e.g., latency), but UDP is often used for time-sensitive applications (e.g., streaming)
        because it avoids delays caused by retransmissions and flow control.
        - Security: Security services, including encryption, can be provided by TLS over TCP. UDP can use DTLS (Datagram Transport Layer Security) to provide similar security features.

    9Q: Recall that TCP can be enhanced with TLS to provide process-to-process security services, including encryption.
    Does TLS operate on the transport layer or the application layer? If the application developer wants TCP to be enhanced
    with TLS what does the developer have to do?
    9A: On the application layer. The developer needs to include TLS code in both the client and server side of the applications.

### 2.2 - 2.5

    10Q: What is meant by a handshaking protocol?
    10A: Processes initiate a handshaking protocol when they want to establish a reliable connection.

    11Q: What does a stateless protocol mean? Is IMAP stateless? Or SMTP?
    11A: A stateless protocol treats each request as an independent transaction with no knowledge of previous requests. No, both IMAP and SMTP are stateful.

    12Q: How can websites keep track of users? Do they always need to use cookies?
    12A: No, some of the methods available (apart from cookies) include: web beacons (small transparent images used to collect data on user engagements such as opening a specific page etc.),
    tracking scripts, ip tracking, browser fingerprinting, etc.

    13Q: Describe how Web caching can reduce the delay in receiving a requested object.
    Will web caching reduce the delay for all objects requested by a user or only for some of the objects? Why?
    13A: Because web cache servers are usually situated geographically near the end user, the reduced distance between the client and the content also reduces the perceived latency. Additionaly they can decrease the load on the main server which can also improve performance. No, it will only reduce the delay for the objects already cached in the cache server, other objects will need to be retrieved from the main server, but (depending on the web cache server strategy) can then potentially be cached for the next user requesting the same objects.

    15Q: Are there any constraints on the format of the HTTP body? What about the email message body sent with SMTP? 
    How can arbitrary data be transmitted over SMTP?
    15A: No, HTTP body can have any format. Email message body is limited to 7-bit ascii encoding. You would have to encode binary information to 7-bit ascii when sending,
    and then decode back to binary on the receiving end.

    16Q: Suppose Alice, with a Web-based e-mail account(such as Hotmail or Gmail) sends a message to Bob, who accesses his mail from his mail server using IMAP.
    Discuss how the message gets from Alice's host to Bob's host. Be sure to list the series of application-layer protocols that are used to move the message 
    between the two hosts.
    16A: 
    - First Alice invokes her user-agent for email. She uses HTTPS to access her user agent (HTTP over TLS).
    - Her user-agent uses SMTP to send the mail to her mail server, where it is placed in a message queue.
    - The client side of the mailing server sees the message in the queue and acquires a TCP connection to a SMTP server running on Bob's mail server.
    - After some initial SMTP handshaking, the SMTP client sends the message into the TCP connection. 
    - At Bob's mail server the server side of SMTP receives the message and places it in Bob's mailbox. 
    - Bob invokes his user-agent to read the message at his convenience. He uses IMAP.

    17Q: Print out the header of an email message you have recently received. How many `Received:` header lines are there?
    Analyze each of the header lines in the message.
    17A: There are multiple `Received` header lines, each server adds its own Received header. Some servers insted add 'X-Received' headers.
    Lines are listed in chronological order from the bottom to the top, so the bottom-most 'X-Received' was the first to be added.
    - line: Received: by 2002:a05:6830:1e6d:b0:6bc:e24c:433a with SMTP id m13csp517038otr;
            Sat, 7 Oct 2023 08:39:03 -0700 (PDT)
    - analyzed: Contains an Ivp6 address of the receiving server + its SMTP ID and the time when the message was received.
    - line: Received: from mail-sor-f41.google.com (mail-sor-f41.google.com. [209.82.109.10])
            by mx.google.com with SMTPS id v36-840002303128312048sdfo09823lfsd023lsdapyo23lk.0.2020.15.08.04.23.99
            for <me@mail.com>
            (Google Transport Security);
            Sat, 07 Oct 2023 08:39:00 -0700 (PDT)
    - analyzed: Contains information on the sending server, the receiving server + its SMTPS ID and the time of receive.
    It indicates that it was transported using Google Transport Security.
    - line: Received-SPF: pass (google.com: domain of info@mail.com designates 209.82.109.10 as permitted sender) client-ip=209.82.109.10;
    - analyzed: Sender Policy Framework tries to prevent email spoofing. The line indicates that the mailing server is permitted to act as a sender 
    on behalf of the email address 'info@mail.com'.
    - line: X-Received: by 2002:a05:6870:1714:b0:1c0:2e8f:17fd with SMTP id h20-20020a056870171400b001c02e8f17fdmr13602271oae.40.1696693138822; Sat, 07 Oct 2023 08:38:58 -0700 (PDT)
    - analyzed: Contains an Ivp6 address of the receiving server + its SMTP ID and the time when the message was received.

    18Q: What is the HOL blocking issue in HTTP/1.1? How does HTTP/2 attempt to solve it? 
    18A: Head Of Line blocking issue occurs when a large request blocks the download of other potentially smaller requests thus the site takes longer to load. 
    HTTP/2 attemps to solve it by multiplexing multiple requests and responses over the same connection.
    
    19Q: Why are MX records needed? Would it not be enough to use a CNAME record? 
    (Assume the email client looks up email addresses through a Type A query and that the target host only runs an email server.)
    19A: Because in that way we can use the same domain name to identify a normal and a mailing server that might have completely different IP addresses.

    20Q: What is the difference between recursive and iterative DNS queries?
    20A: 
        - An iterative DNS query occurs when the client contacts a DNS server, and if the server doesn't know the answer, it returns a referral to another DNS server.
        The client then queries the referred server directly.
        - A recursive DNS query occurs when the client requests a DNS server to resolve the domain name completely.
        The DNS server queries other DNS servers on behalf of the client until it obtains the final IP address and returns it to the client.

### 2.5 
    
    21Q: Under what circumstances is file downloading through p2p much faster than through a centralized client-server approach? 
    Justify your answer using equation:
    F = file size in bits, N = number of peers, ui = upload rate of `i`th peer, us = upload rate of server, d_min = minimal download rate among all (i) peers
    Dp2p = max { F/us, F/d_min, N*F/us + sum(ui, i=1 to N)}
    21A: When there are many peers that are trying to download a file. 
    Let's assume that all peers have the same download speed, additionally: 
    F = 20480 bits, u = 1024 bps, us = 30720 bps, d_min = 10240 bps
            For 300000 peers:
             No P2P time: 200000.0 secs, P2P time in secs: 19.99800019998 secs
            For 500 peers:
             No P2P time: 333.3333333333333 secs, P2P time in secs: 18.867924528301888 secs
            For 5 peers:
             No P2P time: 3.3333333333333335 secs, P2P time in secs: 2.857142857142857 secs
            For 2 peers:
             No P2P time: 1.3333333333333333 secs, P2P time in secs: 2.0 secs
    We can see that even when the server has 30x upload speed of a peer P2P architecture provides noticable efficiency improvements 
    especially when the number of peers rises. 


    22Q: Consider a new peer Alice that joins BitTorrent without possessing any chunks.
    Without any chunks, she cannot become a top-four uploader for any of the other peers,
    since she has nothing to upload. How then will Alice get her first chunk?
    23A: Alice can get randomly selected by a peer to become 'optimistically unchocked' and potentially become one of its preferred peers.
    
    23Q: Assume a BitTorrent tracker suddenly becomes unavailable. What are its consequences? Can files still be downloaded?
    23A: Depending on the user's client the user might be unable to discover new peers 
    (modern client implement distributed hash tables and peer exchange protocol(PEX) to mitigate that).
    Yes, assuming that the initial peer-to-peer download already started communication can continue without the connection to a tracker. 

### 2.6 
    
    24Q: CDNs typically adopt one of two different server placement philosophies. Name and briefly describe them. 
    24A: 
        - Enter Deep: Server clusters are deployed in access ISPs all over the world. The goal is to get closer to end users. Because of the highly distributed design
        maintenance can be challenging.
        - Bring Home: Large clusters at a smaller number of sites. Typically placed in IXPs (Internet Exchange Points).
        Results in lower maintenance and management overhead at the expense of higher delay and lower throughput.

    25Q: Besides network related considerations such as delay, loss and bandwidth performance there are other important factors that go into designing
    a CDN server selection strategy. What are they?
    25A: Geographical proximity, server load and capacity, content type and size, cache efficiency, etc.

### 2.7
    
    26Q: In section 2.7 the UDP server described needed only one socket, whereas the TCP server needed two sockets. Why?
    If the TCP server were to support `n` simultaneous connections, each from a different client host, how many sockets would the TCP server need? 
    26A: Because the UDP does not establish a connection and uses the same socket for all communication. The TCP server opens a new socket to accept each client connection.
    In the case of 'n' simultaneous connections TCP server would need n + 1 sockets.

    27Q: For the client-server application over TCP described in Section 2.7 why must the server program be executed before the client program? 
    For the client-server application over UDP, why may the client program be executed before the server program?
    27A: TCP: Because the client can't do anyhting without first establishing a reliable connection to the server.
    UDP: Since UDP doesn't provide a reliable connection client can send UDP datagrams to the desired address but they may never be received.
    Therefore a client can be started before the server.


## Problems (without solutions)

    P1. True or false?
    a. A user requests a Web page that consists of some text and three images.
    For this page, the client will send one request message and receive four
    response messages.
    b. Two distinct Web pages (for example, www.mit.edu/research
    .html and www.mit.edu/students.html) can be sent over the
    same persistent connection.
    c. With nonpersistent connections between browser and origin server, it is
    possible for a single TCP segment to carry two distinct HTTP request
    messages.
    d. The Date: header in the HTTP response message indicates when the
    object in the response was last modified.
    e. HTTP response messages never have an empty message body.

    P2. SMS, iMessage, Wechat, and WhatsApp are all smartphone real-time mes-
    saging systems. After doing some research on the Internet, for each of these
    systems write one paragraph about the protocols they use. Then write a para-
    graph explaining how they differ.

    P3. Consider an HTTP client that wants to retrieve a Web document at a given URL.
    The IP address of the HTTP server is initially unknown. What transport and application-layer protocols besides HTTP are needed in this scenario?
    P4. Consider the following string of ASCII characters that were captured by Wireshark when the browser sent an HTTP GET message 
    (i.e., this is the actual content of an HTTP GET message).
    The characters <cr><lf> are carriage return and line-feed characters (that is, the italized character string
    <cr> in the text below represents the single carriage-return character that was
    contained at that point in the HTTP header). 
    Answer the following questions, indicating where in the HTTP GET message below you find the answer.

    `GET /cs453/index.html HTTP/1.1<cr><lf>Host: gaia.cs.umass.edu<cr><lf>User-Agent: Mozilla/5.0 (Windows;U; Windows NT 5.1; en-US; rv:1.7.2)
    Gecko/20040804 Netscape/7.2 (ax) <cr><lf>
    Accept:ext/xml, application/xml, application/xhtml+xml, text/html;q=0.9, text/plain;q=0.8,image/png,*/*;q=0.5<cr><lf>
    Accept-Language: en-us,en;q=0.5<cr><lf>
    Accept-Encoding: zip,deflate<cr><lf>
    Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7<cr><lf>
    Keep-Alive: 300<cr><lf>
    Connection:keep-alive<cr><lf><cr><lf>`

    a. What is the URL of the document requested by the browser?
    b. What version of HTTP is the browser running?
    c. Does the browser request a non-persistent or a persistent connection?
    d. What is the IP address of the host on which the browser is running?
    e. What type of browser initiates this message? Why is the browser type needed in an HTTP request message?

    P5. The text below shows the reply sent from the server in response to the HTTP
    GET message in the question above. Answer the following questions, indicat-
    ing where in the message below you find the answer.

    HTTP/1.1 200 OK<cr><lf>Date: Tue, 07 Mar 2008 12:39:45GMT<cr><lf>
    Server: Apache/2.0.52 (Fedora) <cr><lf>
    Last-Modified: Sat, 10 Dec2005 18:27:46 GMT<cr><lf>
    ETag: ”526c3-f22-a88a4c80”<cr><lf>
    Accept-Ranges: bytes<cr><lf>
    Content-Length: 3874<cr><lf>
    Keep-Alive: timeout=max=100<cr><lf>
    Connection: Keep-Alive<cr><lf>
    Content-Type: text/html; charset=ISO-8859-1<cr><lf><cr><lf>
    <!doctype html public ”-//w3c//dtd html 4.0transitional//en”><lf><html><lf>
    <head><lf> <meta http-equiv=”Content-Type” content=”text/html; charset=iso-8859-1”><lf> <meta
    name=”GENERATOR” content=”Mozilla/4.79 [en] (Windows NT 5.0; U) Netscape]”><lf> <title>CMPSCI 453 / 591 / NTU-ST550ASpring 2005 homepage</title><lf></head><lf>
    <much more document text following here (not shown)>

    a. Was the server able to successfully find the document or not? What time
    was the document reply provided?
    b. When was the document last modified?
    c. How many bytes are there in the document being returned?
    d. What are the first 5 bytes of the document being returned? Did the server
    agree to a persistent connection?

    P6. Obtain the HTTP/1.1 specification (RFC 2616). Answer the following
    questions:
    a. Explain the mechanism used for signaling between the client and server
    to indicate that a persistent connection is being closed. Can the client, the
    server, or both signal the close of a connection?
    b. What encryption services are provided by HTTP?
    c. Can a client open three or more simultaneous connections with a given
    server?
    d. Either a server or a client may close a transport connection between them
    if either one detects the connection has been idle for some time. Is it
    possible that one side starts closing a connection while the other side is
    transmitting data via this connection? Explain.

    P7. Suppose within your Web browser you click on a link to obtain a Web page.
    The IP address for the associated URL is not cached in your local host, so
    a DNS lookup is necessary to obtain the IP address. Suppose that n DNS
    servers are visited before your host receives the IP address from DNS; the
    successive visits incur an RTT of RTT1 , . . . , RTTn. Further suppose that the
    Web page associated with the link contains exactly one object, consisting of
    a small amount of HTML text. Let RTT0 denote the RTT between the local
    host and the server containing the object. Assuming zero transmission time
    of the object, how much time elapses from when the client clicks on the link
    until the client receives the object?

    P8. Referring to Problem P7, suppose the HTML file references eight very small
    objects on the same server. Neglecting transmission times, how much time
    elapses with

    a. Non-persistent HTTP with no parallel TCP connections?
    b. Non-persistent HTTP with the browser configured for 6 parallel
    connections?
    c. Persistent HTTP?

    P9. Consider Figure 2.12, for which there is an institutional network connected to
    the Internet. Suppose that the average object size is 1,000,000 bits and that the
    average request rate from the institution’s browsers to the origin servers is 16
    requests per second. Also suppose that the amount of time it takes from when
    the router on the Internet side of the access link forwards an HTTP request until
    it receives the response is three seconds on average (see Section 2.2.5). Model
    the total average response time as the sum of the average access delay (that
    is, the delay from Internet router to institution router) and the average Internet
    delay. For the average access delay, use ∆/(1 - ∆b), where ∆ is the average
    time required to send an object over the access link and b is the arrival rate of
    objects to the access link.
    a. Find the total average response time.
    b. Now suppose a cache is installed in the institutional LAN. Suppose the
    miss rate is 0.4. Find the total response time.

    P10. Consider a short, 10-meter link, over which a sender can transmit at a rate
    of 150 bits/sec in both directions. Suppose that packets containing data
    are 100,000 bits long, and packets containing only control (e.g., ACK or
    handshaking) are 200 bits long. Assume that N parallel connections each
    get 1/N of the link bandwidth. Now consider the HTTP protocol, and suppose
    that each downloaded object is 100 Kbits long, and that the initial downloaded
    object contains 10 referenced objects from the same sender. Would parallel
    downloads via parallel instances of non-persistent HTTP make sense in this
    case? Now consider persistent HTTP. Do you expect significant gains over
    the non-persistent case? Justify and explain your answer.

    P11. Consider the scenario introduced in the previous problem. Now suppose that
    the link is shared by Bob with four other users. Bob uses parallel instances
    of non-persistent HTTP, and the other four users use non-persistent HTTP
    without parallel downloads.
    a. Do Bob’s parallel connections help him get Web pages more quickly?
    Why or why not?
    b. If all five users open five parallel instances of non-persistent HTTP, then
    would Bob’s parallel connections still be beneficial? Why or why not?

    P12. Write a simple TCP program for a server that accepts lines of input from a cli-
    ent and prints the lines onto the server’s standard output. (You can do this by
    modifying the TCPServer.py program in the text.) Compile and execute your
    program. On any other machine that contains a Web browser, set the proxy
    server in the browser to the host that is running your server program; also con-
    figure the port number appropriately. Your browser should now send its GET
    request messages to your server, and your server should display the messages
    on its standard output. Use this platform to determine whether your browser
    generates conditional GET messages for objects that are locally cached.

    P13. Consider sending over HTTP/2 a Web page that consists of one video clip,
    and five images. Suppose that the video clip is transported as 2000 frames,
    and each image has three frames.
    a. If all the video frames are sent first without interleaving, how many
    “frame times” are needed until all five images are sent?
    b. If frames are interleaved, how many frame times are needed until all five
    images are sent.

    P14. Consider the Web page in problem 13. Now HTTP/2 prioritization is
    employed. Suppose all the images are given priority over the video clip, and
    that the first image is given priority over the second image, the second image
    over the third image, and so on. How many frame times will be needed until
    the second image is sent?

    P15. What is the difference between MAIL FROM: in SMTP and From: in the
    mail message itself?

    P16. How does SMTP mark the end of a message body? How about HTTP? Can
    HTTP use the same method as SMTP to mark the end of a message body?
    Explain.

    P17. Read RFC 5321 for SMTP. What does MTA stand for? Consider the follow-
    ing received spam e-mail (modified from a real spam e-mail). Assuming only
    the originator of this spam e-mail is malicious and all other hosts are honest,
    identify the malacious host that has generated this spam e-mail.

    From - Fri Nov 07 13:41:30 2008
    Return-Path: <tennis5@pp33head.com>
    Received: from barmail.cs.umass.edu (barmail.cs.umass.
    edu
    [128.119.240.3]) by cs.umass.edu (8.13.1/8.12.6) for
    <hg@cs.umass.edu>; Fri, 7 Nov 2008 13:27:10 -0500
    Received: from asusus-4b96 (localhost [127.0.0.1]) by
    barmail.cs.umass.edu (Spam Firewall) for <hg@cs.umass.
    edu>; Fri, 7
    Nov 2008 13:27:07 -0500 (EST)
    Received: from asusus-4b96 ([58.88.21.177]) by barmail.
    cs.umass.edu
    for <hg@cs.umass.edu>; Fri, 07 Nov 2008 13:27:07 -0500
    (EST)
    Received: from [58.88.21.177] by inbnd55.exchangeddd.
    com; Sat, 8
    Nov 2008 01:27:07 +0700
    From: ”Jonny” <tennis5@pp33head.com>
    To: <hg@cs.umass.edu>
    Subject: How to secure your savings

    P18. 
    a. What is a whois database?
    b. Use various whois databases on the Internet to obtain the names of two
    DNS servers. Indicate which whois databases you used.
    c. Use nslookup on your local host to send DNS queries to three DNS
    servers: your local DNS server and the two DNS servers you found in
    part (b). Try querying for Type A, NS, and MX reports. Summarize your
    findings.
    d. Use nslookup to find a Web server that has multiple IP addresses. Does
    the Web server of your institution (school or company) have multiple IP
    addresses?
    e. Use the ARIN whois database to determine the IP address range used by
    your university.
    f. Describe how an attacker can use whois databases and the nslookup tool
    to perform reconnaissance on an institution before launching an attack.
    g. Discuss why whois databases should be publicly available.

    P19. In this problem, we use the useful dig tool available on Unix and Linux hosts to
    explore the hierarchy of DNS servers. Recall that in Figure 2.19, a DNS server
    in the DNS hierarchy delegates a DNS query to a DNS server lower in the
    hierarchy, by sending back to the DNS client the name of that lower-level DNS
    server. First read the man page for dig, and then answer the following questions.
    a. Starting with a root DNS server (from one of the root servers [a-m].
    root-servers.net), initiate a sequence of queries for the IP address for your
    department’s Web server by using dig. Show the list of the names of DNS
    servers in the delegation chain in answering your query.
    b. Repeat part (a) for several popular Web sites, such as google.com, yahoo
    .com, or amazon.com.

    P20. Suppose you can access the caches in the local DNS servers of your depart-
    ment. Can you propose a way to roughly determine the Web servers (outside
    your department) that are most popular among the users in your department?
    Explain.

    P21. Suppose that your department has a local DNS server for all computers in the
    department. You are an ordinary user (i.e., not a network/system administra-
    tor). Can you determine if an external Web site was likely accessed from a
    computer in your department a couple of seconds ago? Explain.

    P22. Consider distributing a file of F = 20 Gbits to N peers. The server has
    an upload rate of us = 30 Mbps, and each peer has a download rate of
    di = 2 Mbps and an upload rate of u. For N = 10, 100, and 1,000 and
    u = 300 Kbps, 700 Kbps, and 2 Mbps, prepare a chart giving the minimum
    distribution time for each of the combinations of N and u for both client-
    server distribution and P2P distribution.

    P23. Consider distributing a file of F bits to N peers using a client-server archi-
    tecture. Assume a fluid model where the server can simultaneously transmit
    to multiple peers, transmitting to each peer at different rates, as long as the
    combined rate does not exceed us.
    a. Suppose that us/N … dmin . Specify a distribution scheme that has a distri-
    bution time of NF/us.
    b. Suppose that us/N Ú dmin . Specify a distribution scheme that has a distri-
    bution time of F/dmin .
    c. Conclude that the minimum distribution time is in general given by
    max5NF/us, F/dmin 6.

    P24. Consider distributing a file of F bits to N peers using a P2P architecture.
    Assume a fluid model. For simplicity assume that dmin is very large, so that
    peer download bandwidth is never a bottleneck.
    a. Suppose that us … (us + u1 + . . . + uN)/N. Specify a distribution
    scheme that has a distribution time of F/us.
    b. Suppose that us Ú (us + u1 + . . . + uN)/N. Specify a distribution
    scheme that has a distribution time of NF/(us + u1 + . . . + uN).
    c. Conclude that the minimum distribution time is in general given by
    max5F/us, NF/(us + u1 + . . . + uN)6.

    P25. Consider an overlay network with N active peers, with each pair of peers hav-
    ing an active TCP connection. Additionally, suppose that the TCP connec-
    tions pass through a total of M routers. How many nodes and edges are there
    in the corresponding overlay network?

    P26. Suppose Bob joins a BitTorrent torrent, but he does not want to upload any
    data to any other peers (so called free-riding).
    a. Bob claims that he can receive a complete copy of the file that is shared
    by the swarm. Is Bob’s claim possible? Why or why not?
    b. Bob further claims that he can further make his “free-riding” more
    efficient by using a collection of multiple computers (with distinct IP
    addresses) in the computer lab in his department. How can he do that?

    P27. Consider a DASH system for which there are N video versions (at N different
    rates and qualities) and N audio versions (at N different rates and qualities).
    Suppose we want to allow the player to choose at any time any of the N video
    versions and any of the N audio versions.
    a. If we create files so that the audio is mixed in with the video, so server
    sends only one media stream at given time, how many files will the server
    need to store (each a different URL)?
    b. If the server instead sends the audio and video streams separately and has
    the client synchronize the streams, how many files will the server need to
    store?

    P28. Install and compile the Python programs TCPClient and UDPClient on one
    host and TCPServer and UDPServer on another host.
    a. Suppose you run TCPClient before you run TCPServer. What happens?
    Why?
    b. Suppose you run UDPClient before you run UDPServer. What happens?
    Why?
    c. What happens if you use different port numbers for the client and server
    sides?

    P29. Suppose that in UDPClient.py, after we create the socket, we add the line:
    clientSocket.bind((’’, 5432))
    Will it become necessary to change UDPServer.py? What are the port num-
    bers for the sockets in UDPClient and UDPServer? What were they before
    making this change?

    P30. Can you configure your browser to open multiple simultaneous connections
    to a Web site? What are the advantages and disadvantages of having a large
    number of simultaneous TCP connections?

    P31. We have seen that Internet TCP sockets treat the data being sent as a byte
    stream but UDP sockets recognize message boundaries. What are one
    advantage and one disadvantage of byte-oriented API versus having the API
    explicitly recognize and preserve application-defined message boundaries?

    P32. What is the Apache Web server? How much does it cost? What functional-
    ity does it currently have? You may want to look at Wikipedia to answer this
    question.
