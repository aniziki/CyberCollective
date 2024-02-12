One of the most common ways that web browsers use HTTP cookies is for user authentication and session persistence. Attackers can use Cookies in a malicious way by doing any of the following:
- Stealing cookies that contain sensitive information such as session IDs or authentication cookies.
- Reusing stolen cookies to gain access to authenticated areas and existing user sessions.
- Forging cookies to gain access to authenticated areas and existing sessions.

There are three common attacks that involve cookies and the above malicious actions:
- Cross-site Scripting ([[XSS]]) attacks and man-in-the-middle attacks (MITM) are often used for cookie theft.
- Cross-Site Request Forgery attacks ([[CSRF Attacks]]) abuse the way that browsers handle cookies to perform malicious actions on behalf of authenticated users.

## How to secure cookies?

The HTTP protocol specification contains several mechanisms that developers can use to reduce the risk of attackers accessing, reusing, or forging the contents of the cookies with sensitive data. To use these protections, a developer configures specific parameters along with cookie values when creating new cookies using the Set-Cookie HTTP response header. The parameters affect when cookies are sent back to the server by the browser using the Cookie HTTP request header.

The following are all Set-Cookie HTTP header attributes that can be used to improve cookie security.

## The Expire and Max-Age Attributes

The Expire and Max-Age cookie attributes both define the validity period of the cookie. The Expire attribute sets an absolute date/time of expiration (syntax: `weekday, DD-MM-YYYY hh:mm:ss GMT`), while the Max-Age attribute sets the time limit from the moment the cookie is set.

If the Expire and Max-Age attributes are not set, the browser treats the cookie as a session cookie and deletes it when the browser is closed. If they are set, it treats the cookie as a persistent cookie, stores it client-side, and deletes it according to the values of Expire and Max-Age (whichever comes earlier).

## The Secure attribute

The Secure flag specifies that the cookie may only be transmitted using HTTPS connections (SSL/TLS encryption) and never sent in clear text. If the cookie is set with Secure flag and the browser sends a subsequent request using the HTTP protocol, the web page will not send this cookie to the web server in its HTTP response.

The Secure attribute is meant to protect against man-in-the-middle (MITM) attacks. However, it protects only the confidentiality of the cookie, not its integrity. In a MITM attack, an attacker located between the browser and the server will not receive the cookie from the server via an unencrypted connection but can still send a forged cookie to the server in plain text.

Note that you can only set the Secure flag when using a secure connection.

## The Domain attribute

The Domain attribute declares the domain to which the cookie will be sent. Note that if the domain is set, the cookie will be sent to all its subdomains, too. That means that if you set the domain to example.com, the cookie will also be sent to `www.example.com` and `test.example.com`.

If the Domain attribute is not set, the cookie will only be sent to the original host (without the subdomains), except in the case of Microsoft Internet Explorer, which always sends cookies to subdomains (even when the Domain attribute is not set). Therefore, the most secure way is not to set the Domain attribute unless necessary.

## The Path attribute

The Path attribute declares the URL path to which the cookie will be sent, Note that this includes all subpaths of the declared path. For example, if you set the path to /login/, the cookie will be sent to `example.com/login/` and `example.com/login/admin` but not to `example.com/`.

If you set the PATH attribute to /, the cookie will be sent to all URL paths on the server. If you do not set the Path attribute, the default value is the path from which the cookie was set and all it subpaths. Therefore, the most secure way is not to set the Path attribute unless necessary.

## The HttpOnly attribute

The HttpOnly flag was introduced for [[XSS]] attack mitigation. Without this flag, cookies can be set and read using JavaScript client-side scripts (via document.cookie). This means that if a web application has an XSS vulnerability, an attacker could potentially steal sensitive cookies. Whenever you specify HttpOnly, the browser will send cookies with this flag only in response to HTTP requests.

While the HttpOnly attribute protects the confidentiality of sensitive cookies, it does not protect them from being overwritten. This is because a browser can only store a limited number of cookies for a domain. An attacker may use the [cookie jar overflow attack](https://www.sjoerdlangkemper.nl/2020/05/27/overwriting-httponly-cookies-from-javascript-using-cookie-jar-overflow/) to set a large number of cookies for a domain, deleting the original HttpOnly cookie from browser memory and allowing the attacker to set the same cookie without the flag.

## The SameSite attribute

The SameSite flag instructs web browsers to send cookies differently depending on hnowe a visitor interacts with the site that set the cookie. This flag is used to help protect against [[CSRF Attacks]].

The SameSite cookie attribute may have one of the following values:
- `SameSite=Strict` :The cookie is only sent if you are currently on the site that the cookie is set for. If you are on a different site and click a link to the site that the cookie is set for, the cookie is not send with the first request.
- `SameSite=Lax` : The cookie is not sent for embedded content, but it is sent if you trigger top-level navigation, e.g. by clicking on a link to the site that the cookie is set for. It is sent only with safe request types that do not change state, such as GET.
- `SameSite=None` : The cookie is sent even for embedded content.


Source:
https://www.invicti.com/learn/cookie-security-flags/