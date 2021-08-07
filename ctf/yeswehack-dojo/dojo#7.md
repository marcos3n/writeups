# YesWeHack Dojo Challenge #7

## Description
This XSS challenge has implemented a WAF, that should block all hacking attempts by filtering any character, that would allow the attacker to inject HTML and therefore some `<script>` tags.
The only input is `$config`, but at least it's not a primitive but could also be some complex json structure, that get's parsed into a javascript object.

[Link](https://dojo-yeswehack.com/Playground#eyJkZXNjcmlwdGlvbiI6IiMjIFdBRiBXQUYgLSBET0pPICM3XG5cbl9FdmlsQ29ycDIuMCBoYXMgZGV2ZWxvcGVkIGEgbmV3IFwib3ZlcnNlY3VyZWRuZXh0Z2VuIFdBRlwiIHRvIHByZXZlbnQgYSBtYWxpY2lvdXMgc2NyaXB0IHRvIGJlIGV4ZWN1dGVkLiBUaGV5IHRoaW5rIHRoaXMgV0FGIGNhbiBibG9jayBldmVyeSBwYXlsb2FkcyB0byBhdm9pZHMgWFNTLl9cblxuRG8geW91IHRoaW5rIHlvdSBjYW4gZmluZCBhIHdheSB0byBieXBhc3MgdGhpcyBXQUY/XG5cbioqQlJVVEVGT1JDRSBJUyBOT1QgQUxMT1dFRCoqXG5cbiIsInNvbHV0aW9uIjpudWxsLCJoaW50cyI6W10sInF1ZXJ5Ijp7ImJhY2tlbmQiOiJYU1MiLCJ0ZW1wbGF0ZSI6IjxzY3JpcHQ+XG5jb25zdCBjb25maWcgPSAkY29uZmlnXG5cbmZ1bmN0aW9uIGlzT2JqZWN0KG8pIHtcbiAgICBjb25zdCBwcm90byA9IE9iamVjdC5nZXRQcm90b3R5cGVPZihvKVxuICAgIHJldHVybiB0eXBlb2YgbyA9PT0gXCJvYmplY3RcIiAmJiBwcm90byAhPT0gbnVsbCAmJiBwcm90by5jb25zdHJ1Y3Rvci5uYW1lID09PSBcIk9iamVjdFwiXG59XG5cbmZ1bmN0aW9uIFdBRih4KSB7XG4gICAgY29uc3QgZm9yYmlkZW4gPSBbXCI8XCIsIFwiPlwiLCBcIidcIiwgJ1wiJywgXCI/XCJdXG4gICAgLy8gIE5VTEwgLyBOdW1iZXIgLyBCb29sZWFuXG4gICAgaWYgKHggPT09IG51bGwgfHwgdHlwZW9mIHggPT09IFwibnVtYmVyXCIgfHwgdHlwZW9mIHggPT09IFwiYm9vbGVhblwiKSB7XG4gICAgICAgIHJldHVybiB4XG4gICAgfVxuICAgIC8vIE9iamVjdFxuICAgIGlmIChpc09iamVjdCh4KSkge1xuICAgICAgICByZXR1cm4gT2JqZWN0LmZyb21FbnRyaWVzKE9iamVjdC5lbnRyaWVzKHgpLm1hcCgoW2tleSwgdmFsdWVdKSA9PiBbV0FGKGtleSksIFdBRih2YWx1ZSldKSlcbiAgICB9XG4gICAgLy8gQXJyYXlcbiAgICBpZiAoQXJyYXkuaXNBcnJheSh4KSkge1xuICAgICAgICByZXR1cm4geC5tYXAoeCA9PiBXQUYoeCkpXG4gICAgfVxuICAgIC8vIFN0cmluZ1xuICAgIGZvciAoY29uc3QgbmVlZGxlIG9mIGZvcmJpZGVuKSB7XG4gICAgICAgIGlmICh4LmluY2x1ZGVzKG5lZWRsZSkpIHtcbiAgICAgICAgICAgIHRocm93IG5ldyBFcnJvcihcIkhha2luZyBhdHRlbXB0IGRldGVjdGVkLlwiKVxuICAgICAgICB9XG4gICAgfVxuICAgIHJldHVybiB4XG59XG5mdW5jdGlvbiBnZXRNZXNzYWdlKGNvbmZpZykge1xuICAgIHRyeSB7XG4gICAgICAgIFdBRihjb25maWcpXG4gICAgICAgIE9iamVjdC5zZXRQcm90b3R5cGVPZihjb25maWcsIG51bGwpXG4gICAgICAgIGNvbnN0IG5hbWUgPSBjb25maWcubmFtZSB8fCBcIkd1ZXN0XCJcbiAgICAgICAgcmV0dXJuIGBIZWxsbyAke25hbWV9YFxuICAgIH0gY2F0Y2ggKGUpIHtcbiAgICAgICAgcmV0dXJuIGUubWVzc2FnZVxuICAgIH1cbn1cbmRvY3VtZW50LndyaXRlKGdldE1lc3NhZ2UoY29uZmlnKSlcbjwvc2NyaXB0PiIsImZpbHRlcnMiOnsiJGNvbmZpZyI6W3siZnVuYyI6Ikpzb25pZnkiLCJhcmdzIjpbXX1dfX19) to the challenge

This is the whole code of the challenge:
``` html
<script>
const config = $config

function isObject(o) {
    const proto = Object.getPrototypeOf(o)
    return typeof o === "object" && proto !== null && proto.constructor.name === "Object"
}

function WAF(x) {
    const forbiden = ["<", ">", "'", '"', "?"]
    //  NULL / Number / Boolean
    if (x === null || typeof x === "number" || typeof x === "boolean") {
        return x
    }
    // Object
    if (isObject(x)) {
        return Object.fromEntries(Object.entries(x).map(([key, value]) => [WAF(key), WAF(value)]))
    }
    // Array
    if (Array.isArray(x)) {
        return x.map(x => WAF(x))
    }
    // String
    for (const needle of forbiden) {
        if (x.includes(needle)) {
            throw new Error("Haking attempt detected.")
        }
    }
    return x
}
function getMessage(config) {
    try {
        WAF(config)
        Object.setPrototypeOf(config, null)
        const name = config.name || "Guest"
        return `Hello ${name}`
    } catch (e) {
        return e.message
    }
}
document.write(getMessage(config))
</script>
```

## Analysis

The input we can modify is `$config`. Here are the key points:
1. Before our input `config.name` gets printed, it will be checked by a call to `WAF()`. If our input contains any of the forbidden characters, it will throw an error. The forbidden characters are `["<", ">", "'", '"', "?"]`
2. `WAF` will check any nested attributes in case we provide an array or an object
3. `getMessage` will only print our input, if no error occured.

## Exploitation
After getting the idea, that messing around with javascripts prototype handling could lead to the solution, I looked at the various type checks.

First of all, a simple prototype pollution like this wouldn't work:

``` javascript
const config = {
  "__proto__": {
    "name": "<script>alert(1)<\/script>"
  }
}
```

Although this payload would bypass the WAF check, the prototype itself gets overwritten with `Object.setPrototypeOf(config, null)` right after the WAF check. So the payload has to be a direct member of config.

What caught my attention, was the function `isObject`. It seemed vulnerable, that there were three conditions that must all be true. So, if I could find a way, that at least one of these conditions fails, while `o` actually is an object, it would bypass the checks of all the members.

So my first attempt was to overwrite the constructors name:

``` javascript
const config = {
  "name": "<script>alert(1)<\/script>",
  "__proto__": {
    "constructor": {
      "name": "notanobject"
    }
  }
}
```

Okay. This payload tricked the WAF to skip the check of all objects entries, but we get an exception, because our object doesn't have an `includes` function. I had a quick look at the `jsonify` function and it seems, that it's a simple `JSON.parse` + `JSON.stringify` with some additional replacements. So there is no way to assign a function to `includes` in the proto.

But fortunately there is a special object, that has an `includes` function. It's the `array`. So the next attempt was this one:

``` javascript
const config = {
  "name": "123<script>alert(1)<\/script>",
  "__proto__": []
}
``` 

This time the prototype is an array. So what happens?

1. The function `isObject` returns false because the constructors name is not `Object` but `Array`.
2. This skips the check of `name` as a string, because no member of `config` gets checked.
3. `Array.isArray` returns `false` as well. I didn't fully understand why, but I found some explanations here: http://web.mit.edu/jwalden/www/isArray.html. In short we have something like this: `test(function() { return Array.isArray({ __proto__: Array.prototype }); }, false);`
4. Now includes gets called on an array for every forbidden character. But none is present, because the array is actually empty
5. The WAF couldn't detect any hacking attempt and our XSS-name gets rendered.

## PoC