# ```myTrtl```
myTrtl is a document explaining everything about the Blacket v2 API and how it can be used.

## Tutorial
Structures:   
```
Object - The JSON object sent or received during a request.
    name : type - The property's name and type. If it's an int, it must be wrapped with quotes on ONLY requests to /worker/sell. It's weird, I know.
```

HTTP requests (native JS on browser)   
```js
// POST requests.
fetch('https://v2.blacket.org/my/endpoint', {
    headers: {
        Cookie : 'connect.sid=mysessionid',
        'User-Agent' : 'MYUSERAGENT'
    },
    method : 'POST',
    body   : JSON.stringify({
        key : value
    })
}).then((d) => {
    d.json().then((j) => { // ONLY for endpoints that return something.
        var response = j;
    });
});

// GET requests.
fetch('https://v2.blacket.org/my/endpoint', {
    headers: {
        Cookie : 'connect.sid=mysessionid',
        'User-Agent' : 'MYUSERAGENT'
    },
    method : 'GET',
    body   : null
}).then((d) => {
    d.json().then((j) => { // ONLY for endpoints that return something.
        var response = j;
    });
});
```

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

### Blacklisted User Agents
User agents are headers that specify what type of engine, computer, or software you're running to make a request.   
Many websites know in an advance if you're using an HTTP library that is irregular.   
Blacket does this.

The list of blacklisted user agents Blacket used to use is so:   
```js
var blacklisted = [
    'axios',
    'node-fetch',
    'request',
    'python',
    'java',
    'curl',
    'urllib'
];
```

To change your user agent in Axios, you can use this:
```js
axios.defaults.headers.post['User-Agent'] = 'myuseragenthere';
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
This is an HTTP **get** endpoint.   
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

## ```/logout```
This logs you out. It's a simple HTTP **get** endpoint that returns nothing.

## ```/worker/login```
This is an HTTP **post** endpoint. This logs you in and returns nothing.

Request structure:   
```
Object
    username : string - The user's name.
    password : string - The user's password.
```

## ```/worker/claim```
This is a simple HTTP **get** endpoint. It lets you claim daily tokens.

Response structure:   
```
Object
    error : bool - Has an error occurred? If this is successful, this should be false.
```

## ```/worker/open```
This is an HTTP **post** endpoint that lets you open a pack.

Request structure:   
```
Object
    pack : string - The pack name.
```

Response structure:   
```
Object
    error : bool - Has an error occurred?
    blook : string - The blook's name that you received when you opened the pack.
```

## ```/worker/sell```
This is an HTTP **post** endpoint that lets you sell one-or-more blooks.

Request structure:
```
Object
    blook : string - The blook's name.
    quantity : int 
```

Response structure:
```
Object
    error : bool - Has an error occurred? If successful, this should be false.
```

## ```/worker/news```
This is an HTTP **get** request that lets you get the news.

Response structure:   
```
Object
    error : bool - Has an error occurred?
    news : array<object> - An array of news posts.
        Object
            title : string - The news post's title.
            image : string - The URL of the post's image.
            body  : string - The news post's content.
            date  : int - The news post's date (a JS date).
```
```
