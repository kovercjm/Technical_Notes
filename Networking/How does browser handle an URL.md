# Step by step

![HTTP Transaction 3](https://assets-global.website-files.com/5babb9f91ab233ff5f53ce10/606582264e32b2082d216858_persistent1.png)

1. Resolve IP address from DNS;
2. Establish TCP connection;
3. Send HTTP request;
4. Requested been handled, and Response been returned;
5. Response been rendered in browser;
6. Close TCP connection.

# Key points

## URL

Uniform Resource Locator, with format like the following:

> scheme://host.domain:port/path/filename

- Scheme: protocols like HTTP, HTTPS, FTP, FILE;
- Host, Domain, Port: Location of resource, web page or service;

## DNS

Domain Name System, can have in browser, system, router, ISP, root server.

- A record: resolve a domain to an IPv4 address
- AAAA record: resolve a domain to an IPv6 address
- CNAME record: resolve a domain to a domain
- NS record: resolve a sub-domain with specified DNS
- PTR record: (reversed A record) resolve an IPv4 address to a domain

## Establish and close TCP connection

![image-20210914170626764](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/image-20210914170626764.png)
