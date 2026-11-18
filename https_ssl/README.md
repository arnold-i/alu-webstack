# HTTPS SSL

DNS records, SSL termination and HTTPS redirection for **arnold-i.tech**, using HAProxy on lb-01 in front of two Nginx web servers.

## Servers

| Subdomain | Server | IP |
|---|---|---|
| www.arnold-i.tech | 7302-lb-01 | 13.43.219.47 |
| lb-01.arnold-i.tech | 7302-lb-01 | 13.43.219.47 |
| web-01.arnold-i.tech | 7302-web-01 | 3.10.235.43 |
| web-02.arnold-i.tech | 7302-web-02 | 35.179.115.123 |

## Files

| File | Description |
|---|---|
| `0-world_wide_web` | Bash script that shows the DNS record type and destination for a domain's subdomains (www, lb-01, web-01, web-02), or for one subdomain if given. Uses `dig`, `awk` and a Bash function. |
| `1-haproxy_ssl_termination` | HAProxy config that listens on port 443, terminates SSL with a Let's Encrypt certificate for www.arnold-i.tech, and sends traffic to web-01 and web-02 using roundrobin. |
| `2-redirect_http_to_https` | HAProxy config that does the same and also redirects all HTTP traffic to HTTPS with a 301. |

## Usage

```
$ ./0-world_wide_web arnold-i.tech
The subdomain www is a A record and points to 13.43.219.47
The subdomain lb-01 is a A record and points to 13.43.219.47
The subdomain web-01 is a A record and points to 3.10.235.43
The subdomain web-02 is a A record and points to 35.179.115.123

$ ./0-world_wide_web arnold-i.tech web-02
The subdomain web-02 is a A record and points to 35.179.115.123

$ curl https://www.arnold-i.tech
Holberton School for the win!

$ curl -sI http://www.arnold-i.tech
HTTP/1.1 301 Moved Permanently
location: https://www.arnold-i.tech/
```

## Author

Arnold Ishimwe - a.ishimwe10@alustudent.com
