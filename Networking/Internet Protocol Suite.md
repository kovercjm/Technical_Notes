# OSI model and TCP/IP model

![img](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/1560635630710-comparison-of-osi-and-tcpip.jpg)

![TCP-IP](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/tcp_ip-20210925173958960.png)

# TCP vs. UDP

| TCP                 | UDP              |
| ------------------- | ---------------- |
| connection-oriented | message-oriented |
| reliable            | unreliable       |

- TCP has sliding window flow control;
- TCP has congestion control algorithms, like slow-start, congestion avoidance, fast retransmit and fast recovery;

# Keep-alive Connection

- HTTP keep-alive connection, like DB connection
- Normal API call don't 'keep alive'

# HTTP

## Common request header

- Accept: one or more MIME value that client can accept
- Cookie
- Referer: where the request come from

## Common request method

> GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE, PATCH

Difference between GET and POST is in some browser:

- GET sends Request header with body at once;
- POST sends header, gets `100 Continue`, and then send body

## Common response status code

The first digit of the status code specifies one of five standard classes of responses. All HTTP response status codes are separated into five classes (or categories). The first digit of the status code defines the class of response. The last two digits do not have any class or categorization role. There are five values for the first digit:

 - 1xx (Informational): The request was received, continuing process
 - 2xx (Successful): The request was successfully received, understood and accepted
 - 3xx (Redirection): Further action needs to be taken in order to complete the request
 - 4xx (Client Error): The request contains bad syntax or cannot be fulfilled
 - 5xx (Server Error): The server failed to fulfill an apparently valid request

### Code meaning
Response code | Meaning
------------- | -------
200 OK | Request been done. Return result.
201 Created | Request been done. New resource been created.
204 Accepted | Request accepted. No result.
400 Bad request | The server cannot or will not process the request due to an apparent client error (e.g., malformed request syntax or invalid request message framing
401 Unauthorised | The request was valid, but the server is refusing action, specifically for use when authentication is required and has failed or has not yet been provided.
403 Forbidden | The request was valid, but the server is refusing action.
404 Not found | Requested resources not found.
409 Conflict | Conflict of editing the same resource.
429 Too many request | The user has sent too many requests in a given amount of time.

# HTTPS

![img](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/Encryption-process-Cryptomathic.png)

- Client request TLS connection via TCP;
- Server send its certificate;
- Client validate and send one-time key to Server;
- Communicate via HTTP (symmetric encryption).
