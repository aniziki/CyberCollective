HTTP connections initiate to server port 80 over [[tcp]]. HTTP is stateless, so every transaction is “unique.” 

HTTP Requests occur for all elements or transactions on a webpage; these can be initiated behind the scenes by [[javascript]] calls or on resource/page load. Sites contain links to assets, so when a page loads, the client needs to fetch those assets to display the site properly.

## [Request Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
GET
POST
PUT
DELETE

## [Response Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
HTTP Messages carry specific status codes to inform clients of request statuses
### 1xx
Informational headers
### 2xx
Successful Responses
- 200 - OK
- 201 - Created
- 202 - Accepted
### 3xx
Redirection Messages
- 301 - Moved Permanently
- 302 - Found (temporary move)
- 304 - Not Modified (caching)
- 307 - Temporary Redirect
- 308 - Permanent Redirect
### 4xx
Client Errors
- 400 - Bad Request
- 401 - Unauthorized
- 402 - Payment Required (not implemented)
- 403 - Forbidden
- 404 - File Not Found Error
- 418 - I'm a Teapot (RFC 2324, RFC 7168)
### 5xx
Server Errors
- 500 Internal Server Error
- 501 Not Implemented
- 502 Bad Gateway
- 503 Service Unavailable
- 

## Cookies

## HTTPS

## Caching

#tcp #http