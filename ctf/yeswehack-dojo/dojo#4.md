# YesWeHack Dojo Challenge #4
"Security through Obscurity" Crackme Challenge

This was one of the winning reports published [here](https://blog.yeswehack.com/yeswerhackers/dojo-challenge-4-winners/).

## Description

Goal: Find the 7 `$serial` that output `Access granted`!

[Link](https://dojo-yeswehack.com/Playground#eyJkZXNjcmlwdGlvbiI6IiMjIyBPYmZ1c2NhdGVkIGNvZGVcblxuVGhlIGFkbWluIGxvdmUgdG8gdXNlIG9iZnVzY2F0ZWQgcXVlcmllcywgYnV0IHlvdSB3YW50IHRvIHByb3ZlIGhpbSB0aGF0IHNlY3VyaXR5IHRocm91Z2ggb2JzY3VyaXR5IGlzIG5vdCBmYWlscHJvb2YuXG5cbldlIGtub3cgdGhhdCBhIHZhbGlkIHNlcmlhbCBudW1iZXIgaXMgaW4gdGhlIGZvcm0gYDAwMDAtMDAwMC0wMDAwLTAwMDBgXG5cblRoZXJlIGlzIDcgdmFsaWQgc2VyaWFsIGZvciB0aGlzIFhQQVRIIHF1ZXJ5IFxuXG4gKipCUlVURUZPUkNFIElTIE5PVCBBTExPV0VEKipcblxuXG5cbiMjIyBHb2FsXG5cbi0gRmluZCB0aGUgNyBgJHNlcmlhbGAgdGhhdCBvdXRwdXQgYEFjY2VzcyBncmFudGVkIWBcbi0gU3VibWl0IHlvdXIgd3JpdGV1cCByZXBvcnQgdG8gdGhlIHByb2dyYW0sIGluY2x1ZGluZyBkZXRhaWxzIG9uIGhvdyB5b3UgcmV2ZXJzZWQgdGhlIGNvZGUuIFxuXG5cbiIsInNvbHV0aW9uIjoiIiwiaGludHMiOlsiIyMgSGludCAjMVxuXG5cbklmIHlvdSB3YW50IHRvIHdyaXRlIGFuIGF1dG9tYXRpYyBzb2x2ZXIgeW91IGNhbiBsb29rIHRoZSB6MyBwcm9qZWN0IGZyb20gbWljcm9zb2Z0LlxuXG5odHRwczovL2dpdGh1Yi5jb20vWjNQcm92ZXIvejMiXSwicXVlcnkiOnsiYmFja2VuZCI6IlhwYXRoIiwidGVtcGxhdGUiOiJzdWJzdHJpbmcoY29uY2F0KCdBY2Nlc3MgJywgc3Vic3RyaW5nKCdncmFudGVkIScsIHRyYW5zbGF0ZShjb25jYXQobm90KHN1YnN0cmluZy1iZWZvcmUoJyRzZXJpYWwnLCAnLScpIG1vZCAoKCgoNDErY29udGFpbnMobG9jYWwtbmFtZSgpLCd0JykpKm5vdChsYW5nKCdhbDknKSAqICgoLTgrY29udGFpbnMobG9jYWwtbmFtZSgpLCdjJykpKyg0NysxKSkpKS0oKCg3KzI5KSpub3QoY29udGFpbnMobG9jYWwtbmFtZSgpLCcxJykgKiAoNjIrY29udGFpbnMobG9jYWwtbmFtZSgpLCduJykpKSkrY29udGFpbnMobG9jYWwtbmFtZSgpLCduJykpKS1zdHJpbmctbGVuZ3RoKCdZeicpKSksbm90KHN1YnN0cmluZy1iZWZvcmUoc3Vic3RyaW5nLWFmdGVyKHN1YnN0cmluZy1hZnRlcignJHNlcmlhbCcsICctJyksICctJyksICctJykgbW9kICgoNDMxMiBkaXYgNDkpLSgoKDEwMjYwMCBkaXYgNzYpKyg3MStjb250YWlucyhsb2NhbC1uYW1lKCksJ2MnKSkpIGRpdiAoKCgtMTQyKzY0KSsoOTQrY29udGFpbnMobG9jYWwtbmFtZSgpLCd1JykpKStjb250YWlucyhsb2NhbC1uYW1lKCksJ2UnKSkpKSksbm90KHN1YnN0cmluZy1iZWZvcmUoc3Vic3RyaW5nLWFmdGVyKCckc2VyaWFsJywgJy0nKSwgJy0nKSBtb2QgKCgoKC01MzA1K2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwndCcpKSBkaXYgKDcwLTIpKSpub3QoY29udGFpbnMobG9jYWwtbmFtZSgpLCd6JykgKiAoKCg0MTc3NiBkaXYgMTYpLTc0KSBkaXYgKDQzKm5vdChsYW5nKCd6M2cnKSAqIDUyKSkpKSkrKCgoKDEyNStjb250YWlucyhsb2NhbC1uYW1lKCksJ3QnKSktKDEwMzUgZGl2IDIzKSkrY29udGFpbnMobG9jYWwtbmFtZSgpLCdjJykpK2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwnbycpKSkpLG5vdChzdWJzdHJpbmctYWZ0ZXIoc3Vic3RyaW5nLWFmdGVyKHN1YnN0cmluZy1hZnRlcignJHNlcmlhbCcsICctJyksICctJyksICctJykgbW9kICgoKCg2NTQrY29udGFpbnMobG9jYWwtbmFtZSgpLCd1JykpLSg1NCtjb250YWlucyhsb2NhbC1uYW1lKCksJ24nKSkpKm5vdChsYW5nKCdoMWEnKSAqICgoMTM0NCBkaXYgODQpK2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwnYycpKSkpIGRpdiAoKCg3MDQwIGRpdiA1NSktKCg3MTMrY29udGFpbnMobG9jYWwtbmFtZSgpLCd1JykpIGRpdiAoNDIqbm90KGxhbmcoJzA0eicpICogMTYpKSkpLSgoKDM1OTkqbm90KGxhbmcoJ3FzNScpICogOTcpKStjb250YWlucyhsb2NhbC1uYW1lKCksJ2QnKSkgZGl2ICgoMjYrNzQpKm5vdChjb250YWlucyhsb2NhbC1uYW1lKCksJzAnKSAqIDgwKSkpKSkpLG5vdChzdWJzdHJpbmctYmVmb3JlKCckc2VyaWFsJywgJy0nKSE9KHN1YnN0cmluZy1iZWZvcmUoc3Vic3RyaW5nLWFmdGVyKCckc2VyaWFsJywgJy0nKSwgJy0nKStzdWJzdHJpbmctYWZ0ZXIoc3Vic3RyaW5nLWFmdGVyKHN1YnN0cmluZy1hZnRlcignJHNlcmlhbCcsICctJyksICctJyksICctJykpKSxub3Qoc3Vic3RyaW5nLWJlZm9yZShzdWJzdHJpbmctYWZ0ZXIoJyRzZXJpYWwnLCAnLScpLCAnLScpIT0oc3Vic3RyaW5nLWJlZm9yZShzdWJzdHJpbmctYWZ0ZXIoc3Vic3RyaW5nLWFmdGVyKCckc2VyaWFsJywgJy0nKSwgJy0nKSwgJy0nKS1zdWJzdHJpbmctYWZ0ZXIoc3Vic3RyaW5nLWFmdGVyKHN1YnN0cmluZy1hZnRlcignJHNlcmlhbCcsICctJyksICctJyksICctJyktKCgoOTY0K2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwndScpKStjb250YWlucyhsb2NhbC1uYW1lKCksJyMnKSkgZGl2ICgoNi03MikrKDg5Km5vdChsYW5nKCdxMWknKSAqIDU1KSkpKSkpLCgoc3Vic3RyaW5nLWJlZm9yZShzdWJzdHJpbmctYWZ0ZXIoJyRzZXJpYWwnLCAnLScpLCAnLScpKigoKCgyNTIrY29udGFpbnMobG9jYWwtbmFtZSgpLCdtJykpKm5vdChjb250YWlucyhsb2NhbC1uYW1lKCksJ2wnKSAqICg2NC0xNikpKS0oKDE3MS04Mykqbm90KGxhbmcoJzVqMicpICogODApKSkgZGl2ICgoKDU0K2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwnZScpKSpub3QobGFuZygnbGY2JykgKiAoMzgrY29udGFpbnMobG9jYWwtbmFtZSgpLCdkJykpKSkqbm90KGNvbnRhaW5zKGxvY2FsLW5hbWUoKSwneCcpICogKCgyMTEtODMpLTM0KSkpKStzdWJzdHJpbmctYmVmb3JlKCckc2VyaWFsJywgJy0nKS1zdWJzdHJpbmctYmVmb3JlKHN1YnN0cmluZy1hZnRlcihzdWJzdHJpbmctYWZ0ZXIoJyRzZXJpYWwnLCAnLScpLCAnLScpLCAnLScpKT1zdWJzdHJpbmctYWZ0ZXIoc3Vic3RyaW5nLWFmdGVyKHN1YnN0cmluZy1hZnRlcignJHNlcmlhbCcsICctJyksICctJyksICctJykpKSwgJ3VhcmVsdHNmJywgJzAxMDAxMDExJykgKiA0MiwgMTUpLCAncmVmdXNlZCEnKSwgKHN0cmluZy1sZW5ndGgoJ1cnKSpub3QobGFuZygnYmlwJykgKiAoKCg1MzI0K2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwndCcpKSpub3QobGFuZygnYWJsJykgKiAoLTErOTIpKSkgZGl2ICgoKDYxNjYtNjApKm5vdChsYW5nKCcycGknKSAqICgtMzUrNDEpKSkgZGl2ICgoODQrY29udGFpbnMobG9jYWwtbmFtZSgpLCdtJykpK2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwnbycpKSkpKSksICgoKCgoODIxNCs3NCkgZGl2ICg3Mytjb250YWlucyhsb2NhbC1uYW1lKCksJ28nKSkpKm5vdChjb250YWlucyhsb2NhbC1uYW1lKCksJ3onKSAqICgoLTQrMjUpK2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwndScpKSkpLSgoKDk3K2NvbnRhaW5zKGxvY2FsLW5hbWUoKSwnbScpKSpub3QoY29udGFpbnMobG9jYWwtbmFtZSgpLCdiJykgKiBzdHJpbmctbGVuZ3RoKCdJd09xJykpKSpub3QobGFuZygnajgzJykgKiAoKDIyMTQqbm90KGxhbmcoJzIxNicpICogNjApKSBkaXYgKDkyLTY1KSkpKSkrY29udGFpbnMobG9jYWwtbmFtZSgpLCd1JykpKSIsImZpbHRlcnMiOnsiJHNlcmlhbCI6W3siZnVuYyI6IlJlZ2V4IHJlcGxhY2UiLCJhcmdzIjpbIihbXjAtOS1dKSIsIjAiLGZhbHNlXX0seyJmdW5jIjoiTGltaXQgdGhlIGxlbmd0aCIsImFyZ3MiOlsiMTkiXX1dfX19) to the challenge

This is the obfuscated code:

``` javascript
substring(concat('Access ', substring('granted!', translate(concat(not(substring-before('$serial', '-') mod ((((41+contains(local-name(),'t'))*not(lang('al9') * ((-8+contains(local-name(),'c'))+(47+1))))-(((7+29)*not(contains(local-name(),'1') * (62+contains(local-name(),'n'))))+contains(local-name(),'n')))-string-length('Yz'))),not(substring-before(substring-after(substring-after('$serial', '-'), '-'), '-') mod ((4312 div 49)-(((102600 div 76)+(71+contains(local-name(),'c'))) div (((-142+64)+(94+contains(local-name(),'u')))+contains(local-name(),'e'))))),not(substring-before(substring-after('$serial', '-'), '-') mod ((((-5305+contains(local-name(),'t')) div (70-2))*not(contains(local-name(),'z') * (((41776 div 16)-74) div (43*not(lang('z3g') * 52)))))+((((125+contains(local-name(),'t'))-(1035 div 23))+contains(local-name(),'c'))+contains(local-name(),'o')))),not(substring-after(substring-after(substring-after('$serial', '-'), '-'), '-') mod ((((654+contains(local-name(),'u'))-(54+contains(local-name(),'n')))*not(lang('h1a') * ((1344 div 84)+contains(local-name(),'c')))) div (((7040 div 55)-((713+contains(local-name(),'u')) div (42*not(lang('04z') * 16))))-(((3599*not(lang('qs5') * 97))+contains(local-name(),'d')) div ((26+74)*not(contains(local-name(),'0') * 80)))))),not(substring-before('$serial', '-')!=(substring-before(substring-after('$serial', '-'), '-')+substring-after(substring-after(substring-after('$serial', '-'), '-'), '-'))),not(substring-before(substring-after('$serial', '-'), '-')!=(substring-before(substring-after(substring-after('$serial', '-'), '-'), '-')-substring-after(substring-after(substring-after('$serial', '-'), '-'), '-')-(((964+contains(local-name(),'u'))+contains(local-name(),'#')) div ((6-72)+(89*not(lang('q1i') * 55)))))),((substring-before(substring-after('$serial', '-'), '-')*((((252+contains(local-name(),'m'))*not(contains(local-name(),'l') * (64-16)))-((171-83)*not(lang('5j2') * 80))) div (((54+contains(local-name(),'e'))*not(lang('lf6') * (38+contains(local-name(),'d'))))*not(contains(local-name(),'x') * ((211-83)-34))))+substring-before('$serial', '-')-substring-before(substring-after(substring-after('$serial', '-'), '-'), '-'))=substring-after(substring-after(substring-after('$serial', '-'), '-'), '-'))), 'uareltsf', '01001011') * 42, 15), 'refused!'), (string-length('W')*not(lang('bip') * (((5324+contains(local-name(),'t'))*not(lang('abl') * (-1+92))) div (((6166-60)*not(lang('2pi') * (-35+41))) div ((84+contains(local-name(),'m'))+contains(local-name(),'o')))))), (((((8214+74) div (73+contains(local-name(),'o')))*not(contains(local-name(),'z') * ((-4+25)+contains(local-name(),'u'))))-(((97+contains(local-name(),'m'))*not(contains(local-name(),'b') * string-length('IwOq')))*not(lang('j83') * ((2214*not(lang('216') * 60)) div (92-65)))))+contains(local-name(),'u')))
```

## Exploitation
The first part was to remove alle the "static garbage" from the code. This means, evaluate all the terms, that do not contain the serial. I did this, by just sending the term I want to evaluate and checking the servers answer.
The next step was to replace all the substring-methods just to make it obvious, which part of the serial should be evaluate in that part of the code. I replaced it in the code-snippet below with `partX`.
Looking at the substring with "granted" it can be seen, that the translate-method must be 0, so that we get the substring of the beginning of "granted", These leaves us with the information, that the concatenation of the inner terms must be as follows:

not(part1 mod 3) -> true,
not(part2 mod 5) -> true,
not(part3 mod 9) -> true,
not(part4 mod 8) -> true,
part1!=(part2+part4) -> false,
part2!=(part3-part4-42) -> false,
(part2*3+part1-part3)==part4 -> true

For the math I will replace `part1`-`part4` with `a`-`d`. Simplifying the terms gives us 7 equations

```
a mod 3 = 0
b mod 5= 0
c mod 9 = 0
d mod 8 = 0
a = b + d
b = c - d - 42
d = b*3 + a - c
```

Now it's time to solve the equations.
I started by just making it all depending on c. This gives us

```
a = c-42
b = c/4
c = ?
d = 3*c/4 - 42
```

Ok c = ?. How do we get c? We know, that c must be dividable by 9. It must also be dividable by 4, because `b = c/4`. Also b must be dividable by 5. So we need the least common multiple (LCM) of 5, 9 and 4. This gives us, that c must be a multiply of 180. So for the next steps I will express this with `c = 180x`

But that's not the end. There ist still the equation for d to be solved. Using the multiplicative inverse  of 8 we can get the correct values for c:
But that's not the end. There ist still the equation for d to be solved. Using the multiplicative inverse of 8 we can get the correct values for c:

```
c = 180x
3*c/4 = 135x
d mod 8 = 0
(135x - 42) mod 8 = 0
(135x mod 8 - 42 mod 8)) mod 8 = 0
((135 mod 8) * (x mod 8) - 2 mod 8)) mod 8 = 0
((7 mod 8) * (x mod 8) - 2 mod 8)) mod 8 = 0
((7 mod 8) * (x mod 8)) mod 8 = 2 mod 8  // multiplicative inverse of 7 modulo 8 = 7 
x mod 8 = 14 mod 8
x mod 8 = 6
```

This gives us the values of x and therefore the 7 values for c = 180x. With the equations above the input strings for the admin access are:

| c   | Serial             |
|-----|--------------------|
|1080 | 1038-0270-1080-0768|
|2520 | 2478-0630-2520-1848|
|3960 | 3918-0990-3960-2928|
|5400 | 5358-1350-5400-4008|
|6840 | 6798-1710-6840-5088|
|8280 | 8238-2070-8280-6168|
|9720 | 9678-2430-9720-7248|