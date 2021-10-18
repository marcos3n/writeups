# Combining a feature and a bug to access PII cross origin

## The feature
The site we're talking about was a simple shopping site. There wasn't much extra functionality, so I concentrated my testing on the profile settings. While browsing the site, I noticed, that some endpoints had a JSONP-functionality. 

JSONP is a functionality that allows to share resources cross origin. Instead of returning a simple JSON-object, the server wraps the JSON content into a javascript function. So while the server would e.g. return a JSON like this when requesting `/data`

``` json
{"name": "John"}
```

it would return a javascript body like this when requesting `/data?callback=jsonp`

``` javascript
jsonp({"name":"John"})
```

By wrapping the content into a javascript function and returning a Content-Type `application/javascript`, the browser will allow cross origin accesses.

So in this case it was possible to add the `callback` parameter to the profile request. It would then return a javascript function that contains the JSON data with personally identifiable information about the current user.

But it was not possible to access this data cross origin, because the server made a referrer check. If the referrer was not the shopping site itself, the server responded with an empty body.

# The bug
The first try was to remove the referer header, but the body was still empty. Playing with other headers and the URL revealed an interesting bug: by adding an additional slash to the URL, the server ignored the missing referrer header and returned the users data. So instead of `/data?callback=jsonp` the target URL was now `//data?callback=jsonp`.

# PoC
This is how a simple PoC-page would look like

``` html
<html>
        <head>
        <meta name="referrer" content="no-referrer">
        <script>
                function cb(r) {
                        console.log(r);
                }
        </script>
        <script src="https://example.com//data?callback=jsonp">
        </script>
        </head>
</html>
```