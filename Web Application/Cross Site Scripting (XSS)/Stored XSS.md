[Stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored) (also known as persistent or second-order XSS) arises when an application receives data from an untrusted source and includes that data within its later HTTP responses in an unsafe way.

The data in question might be submitted to the application via HTTP requests; for example, comments on a blog post, user nicknames in a chat room, or contact details on a customer order. In other cases, the data might arrive from other untrusted sources; for example, a webmail application displaying messages received over SMTP, a marketing application displaying social media posts, or a network monitoring application displaying packet data from network traffic.

Here is a simple example of a stored XSS vulnerability. A message board application lets users submit messages, which are displayed to other users:

`<p>Hello, this is my message!</p>`

The application doesn't perform any other processing of the data, so an attacker can easily send a message that attacks other users:

`<p><script>/* Bad stuff here... */</script></p>`

Source:
https://portswigger.net/web-security/cross-site-scripting