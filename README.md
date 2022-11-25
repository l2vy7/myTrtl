# ```myTrtl```
myTrtl is a document explaining everything about the Blacket v2 API and how it can be used.

## General
General API happenings.

### Errors
Errors happen if execution fails for some reason or another.   
While all responses to every endpoint result in status code 200, the information INSIDE the request is what matters.   

If an error occurs, the information is not given to you, but an object is.   
This is the Blacket API error structure.
```
Object
    error : bool - If an error has happened.
    reason : string - The error's reason.
```

Example error handling (JS):

```js
var myendpoint = 'https://v2.blacket.org/worker/user/acai';
var content = {};
fetch(myendpoint, {
        headers : {
            Cookie : 'connect.sid=sessionidhere'
        }
    })
    .then((d) => {
        d.json().then((j) => {
            if (j.error) {
                console.log('An error occured.');
            } else {
                console.log('Success');
            }
            content = j;
        });
    });
```

## HTTP endpoints
HTTP endpoints are API endpoints you can visit on the web. For example, [/worker/user/acai](https://v2.blacket.org/worker/user/acai).

### ```/worker/user/:name```
This gets a user's information by their name or ID.   
If the "name" parameter is left blank, it will give you your information.   
Requesting your own information gives you more info than requesting others' information.

#### Structure of /worker/user/otherPerson
```
Object
    error : bool - Has an error happened?
    user  : object - The user's information.
        id       : int - The user's unique ID.
        username : string - The user's username.
        created  : int - A JS timestamp of when the account was created.
        modified : int - A JS timestamp of when the account was last "online."
        avatar   : string - A relative URL **OR** an absolute URL. Defines the user's avatar.
        banner   : string - A relative URL that defined the user's banner.
        badges   : array<string> - An array of the user's badges.
        blooks   : Object<string, int> - An object containing keys (blook names), and values (blook count).
        tokens   : int - The amount of tokens a user has.
        role     : string - The user's role.
        color    : string - A hex representation (such as #5E30FF) OR a class representation (such as rainbow) of the user's name color.
        exp      : int - The experience points a user has.
```

#### Structure of /worker/user/you or /worker/user/
```
Object
    error : bool - Has an error happened?
    user  : object - The user's information.
        id       : int - The user's unique ID.
        username : string - The user's username.
        created  : int - A JS timestamp of when the account was created.
        modified : int - A JS timestamp of when the account was last "online."
        avatar   : string - A relative URL **OR** an absolute URL. Defines the user's avatar.
        banner   : string - A relative URL that defined the user's banner.
        badges   : array<string> - An array of the user's badges.
        blooks   : Object<string, int> - An object containing keys (blook names), and values (blook count).
        tokens   : int - The amount of tokens a user has.
        role     : string - The user's role.
        color    : string - A hex representation (such as #5E30FF) OR a class representation (such as rainbow) of the user's name color.
        exp      : int - The experience points a user has.
        claimed  : int - A JS timestamp of when the user last claimed their daily tokens.
        perms    : array<string> - An array of the user's permissions.
        friends  : array<string> - Unused, but would've been used for a friends function.
        settings : object - Unused.
            filter : int - Unused.
```
