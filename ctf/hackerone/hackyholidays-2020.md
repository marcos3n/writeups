# Hackerone 12 days of hacky holidays CTF

[Here](https://www.hackerone.com/blog/12-days-hacky-holidays-ctf) is the original announcement and description

## Flag number one
After following the link to the ctf-page, we were presented by a page that does only contain a video. No links, no hints what's inside. No links, no forms, just nothing. So the first thing to do was to check, if someone wants to hide something "obvious". This means, requesting the `robots.txt`. And there was the first flag: `flag{48104912-28b0-494a-9995-a203d1e261e7}`.
 
## Flag number two
The next step was to just follow the path, that lead us to the first flag. Let's have a closer look into the robots.txt:
```
User-agent: *
Disallow: /s3cr3t-ar3a
Flag: flag{48104912-28b0-494a-9995-a203d1e261e7}
```
A secret area!? Yep, it would have been to easy. Similar to the the first page here are no links to follow. Just a hint from the creator, that the page has moved. At first glance there was nothing obvious in the sources. Neither in the html nor in the javascript. What made me curious was, that the mainpage used a jquery.js from google. But this one was using a self hosted one. So there seemed to be something going on in the background. Inspecting the page revealed it. A data-attribute with the flag: `<div class="alert alert-danger text-center" id="alertbox" data-info="flag{b7ebcb75-9100-4f91-8454-cfb9574459f7}" next-page="/apps">`

## Flag number three
Going back to the main page, there was an apps-button. Clicking it and starting the `people-rate` showed a list. Two things were possible here. Loading some more people and getting details for the ones that were already shown. Looking into the requests for the details showed something like this:
`GET https://hackyholidays.h1ctf.com/people-rater/entry?id=eyJpZCI6Mn0=`
A string starting with `ey...` is always a hint for some base64 encoded json. The decoded json looks like this `{"id":2}`. Okay, so the first listed item has the id 2. Let's check the one with the id 1: Creating our own request with the `{"id":1}` encoded as base64 reveals the third flag:

```
GET https://hackyholidays.h1ctf.com/people-rater/entry?id=eyJpZCI6MX0=

{
  "id": "eyJpZCI6MX0=",
  "name": "The Grinch",
  "rating": "Amazing in every possible way!",
  "flag": "flag{b705fb11-fb55-442f-847f-0931be82ed9a}"
}
```

## Flag number four
A new app has been released. The swag-shop. Trying to purchase some swag presented a login mask. Poking araund with some sql-injection payloads didn't bring any results. But there were some api calls. So the next intention was to find some hidden api endpoints. Running a small wordlist gave me the user endpoint. But there was an error: `Bad Request ... {"error": "Missing required fields"}`. Okay. So the next step must be to find one or more valid parameters. Manually trying some like id and name didn't bring up anything. So I needed the next wordlist: `uuid`. Could've guessed that...
But where should I get the uuid? Looking through the sources to find any uuid leakage was just some waste of time. After a while I ended up using just a bigger wordlist for the api-endpoints. And there it was: `api/sessions`. The third base64-encoded session brought up a uuid. And with that one, it was just one more api call to get all the details of the grinch, including the next flag:
```
{
  "uuid": "C7DCCE-0E0DAB-B20226-FC92EA-1B9043",
  "username": "grinch",
  "address":   {
    "line_1": "The Grinch",
    "line_2": "The Cave",
    "line_3": "Mount Crumpit",
    "line_4": "Whoville"
  },
  "flag": "flag{972e7072-b1b6-4bf7-b825-a912d3fd38d6}"
}
```

## Flag number five
The secure login app is protected by a login page. The first step was to just check the login functionality. I used some dummy values to check the request and the response. The responses error code was quite a bit to detailed. It showed the hint, that the username was wrong. That makes a brute-force attack somewhat easier. Hoping that there is no rate-limiting implemented, I started a scan with some names. With the name `access` the response error changed to `Invalid password`. That means, that I now know the username. With another wordlist containing passwords, it was only some seconds more, to get past the login page: `access: computer`.
Being logged in, there were no files to download. A quick look at the base64 encoded cookie reveals that I'm not an admin:
`{"cookie":"1b5e5f2c9d58a30af4e16a71a45d0172","admin":false}`
Saving user data in an unsigned cookie is always a bad idea, because the server can't check if some of the fields have been manipulated. So we do. Changing the admin flag to true gives us a zip to download: `my_secure_files_not_for_you.zip`.
Unfortunately the files are protected by another password. There is a `flag.txt` and a `xxx.png`. Downloading and running `fcrackzip` against it, gives the password `hahahaha` and the flag inside the flag.txt: `flag{2e6f9bf8-fdbd-483b-8c18-bdf371b2b004}`.

## Flag number six
In this challenge, we have access to the grinchs diary. The first thing that stands out, is the parameter name: `template`. This is always a good indicator for a LFI. While checking some files to include, I found, that adding `index.php` as template value revealed the php code. Reading it gives a hint, that there is another php file: `secretadmin.php`. My first action was to call the file. But the only thing that was responded, was an error syaing, that the page is not acceible from my ip. This made me think, I had to bypass the ip filter and I played a bit with the X-Forwarded-For and similar headers, but didn't get any valuable result. So I went back to the index.php and looked more into the code. Looking in detail into the replacements, it was clear, that it was an easy thing to bypass it: (I added some spaces to make the different part more visible.
```
secretad  secretadm  admin.php  in.php  min.php

// The first replacement removes the admin.php in the middle. That leaves the folling:
secretad  secretadmin.php  min.php

// The second replacement removes the inner part and leaves:
secretadmin.php

This gives the sixth flag: flag{18b130a7-3a79-4c70-b73b-7f23fa95d395}
```

## Flag number seven
The grinch has build up a brand new hate-mail-generator. With this generator it's possible to prepare some templates and use them to send a lot of mails to different people. As anonymous user, it's only possible to build a template and show the preview. Looking at the one and only stored template, we get some info about the possibilities we have to build a template. First: we can set some variables for e.g. the name and the email. Second: we can include some html templates.
The first thing I tried was to test for path traversal, but seemed to be filtered similar to the diary app. Next I thought, what about nesting variables? So I tried to set the name value to `{{name}}`. But this nested variable wasn't replaced. Next I tried the same with another template: setting name to `{{template:cbdj3_grinch_footer.html}}`. And this was handled differently. The "nested" template was also included. So I tried some path traversal tricks here as well. It doesn't work either, but I also noticed, that there was no error message, so it seemed that those "nested templates" were handled differently. I tried different things but was stuck at this point. After a short break I thought about the error message, if we set some template, that does not exists. It reveals the templates' path: `/templates`. Good luck, that directory listing was enabled for this directory. There is another template: `38dhs_admins_only_header.html`.
Using this in a normal template variable gives an error, that we are not allowed to use it. But as I found out above, the processing of "nested" template was different. So setting the name to `{{template:38dhs_admins_only_header.html}}` revealed the admin template and the flag: `flag{5bee8cf2-acf2-4a08-a35f-b48d5e979fdd}`.

This is the final request:
```
preview_markup=Hi+%7B%7Bname%7D%7D.....+Guess+what.....+%3Cstrong%3EYOU+SUCK%21%3C%2Fstrong%3E&preview_data=%7B%22name%22%3A%22%7B%7Btemplate%3a38dhs_admins_only_header.html%7D%7D%22%2C%22email%22%3A%22alice%40test.com%22%7D
```

## Flag number nine
In the evil-quiz app we can check, how evil we are. Doing the quiz starts with entering a name, answering the questions and getting the score. Looking at the score there is a hint, how many other participants used the same name. This looks like a possibility for some SQL-Injection.

Trying a first payload succeeds: `'or''='` as name gives us a big number of other users, that uses that name. Thus it's possible to add some where clauses to make it a boolean based blind SQLi. The next step was to get the table name. I used a query like that to get the name (sprintf format):
`' and 0<(SELECT COUNT(table_name) FROM information_schema.tables where table_name like '%s%%');--`
I have written a small script, that first sets the name and second checks the score-request for the number of "same names". 0 means the query failed, !=0 means the query was successful. This gives us the table `admin`. We can do the same for the column names and get `username` and `password`.
Knowing the colums, it's possible to get the exact values for the username and the password. I modified the query a little bit to get case sensitive values: `' and 0<(SELECT COUNT(password) FROM admin where password like binary '%s%%' ESCAPE '#');--`

This gives us the admin credentials `admin:S3creT_p4ssw0rd-$` and the flag: `flag{6e8a2df4-5b14-400f-a85a-08a260b59135}`