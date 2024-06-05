## QUESTIONS:

The following text contains questions and answers. Each answer needs to provide a satisfactory response to the corresponding question. Verify the validity of each answer.

2.1

    1q. List five nonproprietary Internet applications and the application layer protocols that they use. 
    1a. Web - HTTP 1.1, Email - SMTP, IMAP and POP3, File transfer - FTP, SFTP (Secure File Transfer), Streaming multimedia - HTTP / DASH, Remote terminal access - Telnet, SSH

    2q. What is the difference between network architecture and application architeture?
    2a. Network architecture refers to the different layers (Application, Transport, Network, Link, Physical) that enable various methods of network communications.
    Application architecture is designed by application developer and dictates how the application is structured on various end systems.

    3q. For a communication session between a pair of processes, which process is the client and which the server?
    3a. The process that initiates the communication is the client and the process responding is the server.

    4q. Why are the terms client and server still used in p2p applications?
    4a. Because each peer is acting both as a client and as a server, potentially at the same time.

    5q. What info is used by a process running on one host to identify a process running on another host?
    5a. IP address and port number.

    6q. What is the role of HTTP in a network application? What other components are needed to complete a Web app?
    6a. It provides semantics enabling multiple autonomous developers to develop client and server applications that can communicate with each other.
    A web server software, client side software (also potentially a database server).

    7q. Referring to Figure 2.4, we see that none of the applications listed requires both no data loss and timing.
    Can you conceive of an application that requires no data loss and that is also highly time-sensitive?
    7a. Trading bot / an exchange.

    8q. List the four broad classes of services that a transport protocol can provide. For each of the service classes,
    indicate if either UDP or TCP (or both) provides such a service.
    8a. - Reliable data transfer: Provided by TCP. TCP ensures that data is delivered accurately and in order, retransmitting lost packets if necessary.
        - Throughput: TCP can provide guaranteed throughput to some extent, as it manages congestion control and flow control to optimize data flow.
        UDP does not provide guaranteed throughput, but it can achieve higher throughput in applications where occasional data loss is acceptable because it has less overhead.
        - Timing: Neither TCP nor UDP inherently guarantees timing (e.g., latency), but UDP is often used for time-sensitive applications (e.g., streaming)
        because it avoids delays caused by retransmissions and flow control.
        - Security: Security services, including encryption, can be provided by TLS over TCP. UDP can use DTLS (Datagram Transport Layer Security) to provide similar security features.

    9q. Recall that TCP can be enhanced with TLS to provide process-to-process security services, including encryption.
    Does TLS operate on the transport layer or the application layer? If the application developer wants TCP to be enhanced
    with TLS what does the developer have to do?
    9a. On the application layer. The developer needs to include TLS code in both the client and server side of the applications.

2.2 - 2.5

    10q. What is meant by a handshaking protocol?
    10a. Processes initiate a handshaking protocol when they want to establish a reliable connection.

    11q. What does a stateless protocol mean? Is IMAP stateless? Or SMTP?
    11a. A stateless protocol treats each request as an independent transaction with no knowledge of previous requests. No, both IMAP and SMTP are stateful.

    12q. How can websites keep track of users? Do they always need to use cookies?
    12a. No, some of the methods available (apart from cookies) include: web beacons (small transparent images used to collect data on user engagements such as opening a specific page etc.),
    tracking scripts, ip tracking, browser fingerprinting, etc.

    13q. Describe how Web caching can reduce the delay in receiving a requested object.
    Will web caching reduce the delay for all objects requested by a user or only for some of the objects? Why?
    13a. Because web cache servers are usually situated geographically near the end user, the reduced distance between the client and the content also reduces the perceived latency. Additionaly they can decrease the load on the main server which can also improve performance. No, it will only reduce the delay for the objects already cached in the cache server, other objects will need to be retrieved from the main server, but (depending on the web cache server strategy) can then potentially be cached for the next user requesting the same objects.

    15q. Are there any constraints on the format of the HTTP body? What about the email message body sent with SMTP? 
    How can arbitrary data be transmitted over SMTP?
    15a. No, HTTP body can have any format. Email message body is limited to 7-bit ascii encoding. You would have to encode binary information to 7-bit ascii when sending,
    and then decode back to binary on the receiving end.

    16q. Suppose Alice, with a Web-based e-mail account(such as Hotmail or Gmail) sends a message to Bob, who accesses his mail from his mail server using IMAP.
    Discuss how the message gets from Alice's host to Bob's host. Be sure to list the series of application-layer protocols that are used to move the message 
    between the two hosts.
    16a. 
    - First Alice invokes her user-agent for email. She uses HTTPS to access her user agent (HTTP over TLS).
    - Her user-agent uses SMTP to send the mail to her mail server, where it is placed in a message queue.
    - The client side of the mailing server sees the message in the queue and acquires a TCP connection to a SMTP server running on Bob's mail server.
    - After some initial SMTP handshaking, the SMTP client sends the message into the TCP connection. 
    - At Bob's mail server the server side of SMTP receives the message and places it in Bob's mailbox. 
    - Bob invokes his user-agent to read the message at his convenience. He uses IMAP.

    17q. Print out the header of an email message you have recently received. How many `Received:` header lines are there?
    Analyze each of the header lines in the message.
    17a. There are multiple `Received` header lines, each server adds its own Received header. Some servers insted add 'X-Received' headers.
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

    18q. What is the HOL blocking issue in HTTP/1.1? How does HTTP/2 attempt to solve it? 
    18a. Head Of Line blocking issue occurs when a large request blocks the download of other potentially smaller requests thus the site takes longer to load. 
    HTTP/2 attemps to solve it by multiplexing multiple requests and responses over the same connection.
    
    19q. Why are MX records needed? Would it not be enough to use a CNAME record? 
    (Assume the email client looks up email addresses through a Type A query and that the target host only runs an email server.)
    19a. Because in that way we can use the same domain name to identify a normal and a mailing server that might have completely different IP addresses.

    20q. What is the difference between recursive and iterative DNS queries?
    20a. - An iterative DNS query occurs when the client contacts a DNS server, and if the server doesn't know the answer, it returns a referral to another DNS server.
        The client then queries the referred server directly.
        - A recursive DNS query occurs when the client requests a DNS server to resolve the domain name completely.
        The DNS server queries other DNS servers on behalf of the client until it obtains the final IP address and returns it to the client.

VALID

2.5 
    
    21q. Under what circumstances is file downloading through p2p much faster than through a centralized client-server approach? 
    Justify your answer using equation:

    22q. Consider a new peer Alice that joins BitTorrent without possessing any chunks.
    Without any chunks, she cannot become a top-four uploader for any of the other peers,
    since she has nothing to upload. How then awill Alice get her first chunk?
    
    23q. Assume a BitTorrent tracker suddenly becomes unavailable. What are its consequences? Can files still be downloaded?

2.6 
    
    24q. CDNs typically adopt one of two different server placement philosophies. Name and briefly describe them. 

    25q. Besides network related considerations such as delay, loss and bandwidth performance there are other important factors that go into designing
    a CDN server selection strategy. What are they?

2.7
    
    26q. In section 2.7 the UDP server described needed only one socket, whereas the TCP server needed two sockets. Why?
    If the TCP server were to support `n` simultaneous connections, each from a different client host, how many sockets would the TCP server need? 

    27q. For the client-server application over TCP described in Section 2.7 why must the server program be executed before the client program? 
    For the client-server application over UDP, why may the client program be executed before the server program?



