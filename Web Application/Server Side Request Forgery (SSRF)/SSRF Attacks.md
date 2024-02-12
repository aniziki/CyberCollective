A Server-Side Request Forgery (SSRF) vulnerability occurs when an attacker manipulates a server-side application into making HTTP requests to a domain of their choice. This vulnerability exposes the server to arbitrary external requests directed by the attacker.

In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems. This could leak sensitive data, such as authorization credentials.

Source:
https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery
https://portswigger.net/web-security/ssrf