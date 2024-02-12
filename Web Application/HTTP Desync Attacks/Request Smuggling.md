The vulnerability occurs when a desynchronization between the front-end proxies and the back-end server allows an attacker to send an HTTP request that will be interpreted as a single request by the front-end proxies (load balance/reverse-proxy) and as a 2 request by the back-end server. This allows a user to modify the next request that arrives to the back-end server after his.

Source:
https://book.hacktricks.xyz/pentesting-web/http-request-smuggling