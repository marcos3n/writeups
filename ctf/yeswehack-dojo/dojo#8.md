# YesWeHack Dojo Challenge #8

## Description
In this challenge we're supposed to bypass a WAF, that shall prevent us from accessing a secret. The provided functionality is a simple server side script, that requests an http resource internally.

[Link](https://dojo-yeswehack.com/Playground#eyJkZXNjcmlwdGlvbiI6IiMgWU9VIEN+flN+fkhBTEwgTk9UIFBBU1MgLSBET0pPICM4XG4jIyBDaGFsbFxuXG5fRXZpbENvcnAyLjAgaGFzIGEgc2NyaXB0IHRvIGNoZWNrIHRoZSBjdXJyZW50IHN0YXRlIG9mIGFuIGludGVybmFsIHNlcnZpY2UuIFRoZXkgd2FudCB0byBtYWtlIHN1cmUgdGhhdCB0aGlzIHNjcmlwdCwgd2l0aCBhbGwgaXRzIHNlY3VyaXR5LCBjYW5ub3QgYmUgdXNlZCB0byByZXRyaWV2ZSB0aGUgKipzZWNyZXQqKiBwYXRoLi5fXG5cbklmIHlvdSBmaW5kIGEgd2F5IHRvIGdldCB0aGUgc2VjcmV0IHVzaW5nIHRoaXMgc2NyaXB0LCBsZXQgdXMga25vdyFcblxuIyMgR29hbFxuXG5GaW5kIGEgd2F5IHRvIGJ5cGFzcyBhbGwgc2VjdXJpdHkgbWVjaGFuaXNtcyB0byByZXRyaWV2ZSB0aGUgYC9zZWNyZXRgLlxuXG4qKkJSVVRFRk9SQ0UgSVMgTk9UIEFMTE9XRUQqKiIsInNvbHV0aW9uIjpudWxsLCJoaW50cyI6WyIjIyBIaW50ICMxXG5cbllvdSBjYW4gZmluZCB0aGUgc2VjcmV0IG9uIGxvY2FsaG9zdCBwb3J0IDUwMDAgYW5kIHBhdGggYC9zZWNyZXRgIl0sInF1ZXJ5Ijp7ImJhY2tlbmQiOiJEZW5vIiwidGVtcGxhdGUiOiJjb25zdCB1cmwgPSAkdXJsXG5cbmZ1bmN0aW9uIHdhZihzdHIpe1xuICBjb25zdCB1cmwgPSBuZXcgVVJMKHN0cilcbiAgaWYgKHN0ciA9PT0gXCJodHRwOi8vMTI3LjAuMC4xOjUwMDAvcGluZ1wiKXtcbiAgICByZXR1cm5cbiAgfVxuICBpZiAodXJsLnBvcnQgIT09ICc1MDAwJyl7XG4gICAgdGhyb3cgbmV3IEVycm9yKFwiV0FGOiBJbnZhbGlkIHBvcnRcIilcbiAgfVxuICBpZiAodXJsLmhvc3RuYW1lID09PSBcIjEyNy4wLjAuMVwiIHx8IHVybC5ob3N0bmFtZSA9PT0gXCJsb2NhbGhvc3RcIil7XG4gICAgdGhyb3cgbmV3IEVycm9yKFwiV0FGOiBJbnZhbGlkIG9yaWdpblwiKVxuICB9XG4gIGlmICghdXJsLnBhdGhuYW1lLnN0YXJ0c1dpdGgoXCIvcGluZ1wiKSl7XG4gICAgdGhyb3cgbmV3IEVycm9yKFwiV0FGOiBJbnZhbGlkIGZvbGRlclwiKVxuICB9XG4gIGlmICgvXFwvcGluZy4qXFwvLy5leGVjKHN0cikpe1xuICAgIHRocm93IG5ldyBFcnJvcihcIldBRjogSW52YWxpZCBwYXRoXCIpXG4gIH1cbn1cblxudHJ5IHtcbiAgd2FmKHVybClcbiAgY29uc3QgciA9IGF3YWl0IGZldGNoKHVybClcbiAgY29uc3QgcG9uZyA9IGF3YWl0IHIudGV4dCgpXG4gIGNvbnNvbGUubG9nKHBvbmcpXG59IGNhdGNoKGUpIHtcbiAgY29uc29sZS5sb2coZS5tZXNzYWdlKVxufVxuIiwiZmlsdGVycyI6eyIkdXJsIjpbeyJmdW5jIjoiU3RyaW5naWZ5IiwiYXJncyI6W2ZhbHNlXX1dfX19) to the challenge

This is the whole code of the challenge:

``` javascript
const url = $url

function waf(str){
  const url = new URL(str)
  if (str === "http://127.0.0.1:5000/ping"){
    return
  }
  if (url.port !== '5000'){
    throw new Error("WAF: Invalid port")
  }
  if (url.hostname === "127.0.0.1" || url.hostname === "localhost"){
    throw new Error("WAF: Invalid origin")
  }
  if (!url.pathname.startsWith("/ping")){
    throw new Error("WAF: Invalid folder")
  }
  if (/\/ping.*\//.exec(str)){
    throw new Error("WAF: Invalid path")
  }
}

try {
  waf(url)
  const r = await fetch(url)
  const pong = await r.text()
  console.log(pong)
} catch(e) {
  console.log(e.message)
}
```

## Analysis / Exploitation

We can see, that the WAF makes multiple checks on the provided url. If the check succeeds, the provided URL will be fetched and the response body printed back to the user.

The first check is a comparison to the URL `http://127.0.0.1:5000/ping`. If we provide this URL, the WAF returns and we get a `pong` printed as result.
If we provide any other URL, the port of our request will be checked:

``` javascript
if (url.port !== '5000'){
    throw new Error("WAF: Invalid port")
}
```

That seems pretty straight forward. We will have to provide an URL with the port 5000. But luckily the secret can be found on this port. So that shouldn't stop us.

Having a valid port, the origin gets checked:

``` javascript
if (url.hostname === "127.0.0.1" || url.hostname === "localhost"){
    throw new Error("WAF: Invalid origin")
}
```

Okay, this is the first obstacle. If we don't provide the above URL, we're not allowed to access localhost. Or are we!? There are some possibilities to bypass this check:

* The localhost address range is `127.0.0.0/8`. So we can just use one of the 16.777.213 other addresses like `127.0.0.2`
* Or we can use the 0 address like this: `http://0:5000`
* Other things I tested don't work because of the deno-restrictions: ipv6 like this `http://[::1]:5000` and external dns resolution

The next check is about the path. It must start with /ping:

``` javascript
if (!url.pathname.startsWith("/ping")){
    throw new Error("WAF: Invalid folder")
}
```

Because the check is not made on the original string but on the parsed URL object, a simple path traversal attack like `/ping/../secret` does not work. The URL-parsing normalises the path. So what we have to do, is to prevent the path-normalization. This can be done by URL-encoding one of the dots like this: `/ping/.%2e/secret`.

The last check shall prevent a path traversal attack by checking that no slash appears after the /ping:

``` javascript
if (/\/ping.*\//.exec(str)){
    throw new Error("WAF: Invalid path")
}
```

The first thing to note is, that this check is also about the path (like the one above), but uses another base. This check is not done on the path of the parsed URL but on the original string. So we can take advantage of he path normalization: `\` becomes `/`. To bypass this check, we change the path from `/ping/.%2e/secret` to `/ping\.%2e\secret`.

## PoC
Eventually, we get the secret by providing this URL:
`http://127.0.0.2:5000/ping\.%2e\secret`