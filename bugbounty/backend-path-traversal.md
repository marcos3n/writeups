# Path traversal in backend api requests
This path traversal bug allows an attacker to access any backend endpoint and modify or leak company and user data.

## Discovery
The target was an application of a telecom provider for business customers. It allows the customers to manage sites, internet connections, sim cards, users' documents and much more.
The first time I discovered this kind of bug was a download functionality for tutorial documents. It looked like this:

```
https://example.com/tutorial/document/dl?id_document=10
```

I guess in the background the frontend server made a request to the backend like this:

```
https://backend.internal/api/client/123/company/documents/10
```

So by going up some directories we can control what the API request looks like this:

```
https://example.com/tutorial/document/dl?id_document=10/../../../../../../api/client/

->

https://backend.internal/api/client/123/company/documents/10/../../../../../../api/client/
```

This returned the details of all companies. Analyzing other frontend requests it was possible to add subfolders and to get additional details like users, contracts, documents and tickets.

## Escalation
While this was a GET request and it was possible to retrieve other companies' data, I wondered if this bug exists in other places and would allow manipulation of data.

Looking around I found that the PUT request to update a user was also vulnerable. In this case, it was the `id` JSON parameter in the requests body.

```
PUT /administration/user/update HTTP/1.1
...

{
    "user": {
        "id": 123,
        "name": "Marco",
        "firstnam": "Sen",
        "email": "any@example.com"
    }
}
```

By setting the id to `"x/../../../COMPANY_ID/user/123"` it was possible to update the user as well. Eventually, this bug allows an attacker to modify any user and get access to any other company as an admin.