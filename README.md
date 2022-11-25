# ```myTrtl```
myTrtl is a document explaining everything about the Blacket v2 API and how it can be used.

## HTTP endpoints
HTTP endpoints are API endpoints you can visit on the web. For example, [/worker/user/acai](https://v2.blacket.org/worker/user/acai).

### ```/worker/user/:name```
This gets a user's information by their name or ID.   
If the "name" parameter is left blank, it will give you your information.   
Requesting your own information gives you more info than requesting others' information.

#### Structure of /worker/user/otherPerson
```
Object
    error : bool
    user  : object
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
    error : bool
    user  : object
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
