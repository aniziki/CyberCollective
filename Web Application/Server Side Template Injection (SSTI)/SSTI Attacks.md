A Server-Side Template Injection (SSTI) vulnerability occurs when an attack can inject malicious code into a template that is executed on the server. This vulnerability can be found in various technologies, including Jinja.

Jinja is a popular template engine used in web applications. Let's consider an example that demonstrates a vulnerable code snipped using Jinja:
`output = template.render(name=request.args.get('name'))`

In this vulnerable code, the `name` parameter from the user's request is directly passed into the template using the `render` function. This can potentially allow an attacker to inject malicious code into the `name` parameter, leading to server-side template injection.

For instance, an attacker could craft a request with a payload like this: 
`http://vulnerable-website.com/?name={{bad-stuff-here}}`

The payload `{{bad-stuff-here}}` is injected into the `name` parameter. This payload can contain Jinja template directives that enable the attacker to execute unauthorized code or manipulate the template engine, potentially gaining control over the server.

## Detection

To detect Server-Side Template Injection (SSTI), initially, fuzzing the template is a straightforward approach. This involves injecting a sequence of special characters (`${{<%[%'"}}%\`) into the template and analyzing the differences in the server's response to regular data verses this special payload. Vulnerability indicators include: 
- Thrown errors, revealing the vulnerability and potentially the template engine
- Absence of the payload reflection, or parts of it missing, implying the server processes it differently than regular data.
- Plaintext Context: Distinguish from XSS by checking if the server evaluates template expressions (e.g. `{{7*7}}` , `${7*7}` ).
- Code Context: Confirm vulnerability by altering input parameters. For instance, changing `greeting` in `http://vulnerable-website.com/?greeting=data.username` to see if the server's output is dynamic or fixed, like in `greeting=data.username}}hello` returning the username.

## Identification Phase

Identifying the template engine involves analyzing error messages or manually testing various language-specific payloads. Common payloads causing errors include `${7/0}`, `{{7/0}}`, and `<%= 7/0 %>`. Observing the server's response to mathematical operations helps pinpoint the specific template engine.


Source: 
https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection