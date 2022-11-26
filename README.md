# ```myTrtl```
myTrtl is a document explaining everything about the Blacket v2 API and how it can be used.

This document can be used so you can make your own Trtl!

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
        headers: {
            Cookie : 'connect.sid=sessionidhere'
        },
        method : 'GET',
        body   : null
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

To get the session ID, set the Cookie header to a blank string and extract the Session ID like so:   
```js
fetch('https://v2.blacket.org/worker/login', {
    headers: {
        Cookie : 'connect.sid=sessionidhere'
    },
    method : 'GET',
    body   : null
}).then((d) => {
    var sessionID = d.headers.get('Set-Cookie').split(';')[0].replace('connect.sid=', '');
});
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
    blook    : string - The blook's name.
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
    news  : array<object> - An array of news posts.
        Object
            title : string - The news post's title.
            image : string - The URL of the post's image.
            body  : string - The news post's content.
            date  : int - The news post's date (a JS date).
```

## ```/worker/set```
This is an HTTP **post** request that sets your PFP or banner.

Request structure:   
```
Object
    type : string - Either 'blook' or 'banner'
    banner : string - If the type is 'banner', this is the name of the banner.
    blook : string - If the type is 'blook', this is the name of the blook.
```

Response structure:
```
Object
    error : bool - Has an error occurred? If successful, this should be false.
```

## ```/worker/change```
This is an HTTP **post** request that changes your username or password.

Request structure:   
```
Object
    type : string - Either 'username' or 'password'
    username : string - If the type is 'username', this is your NEW username.
    password : string - If the type is 'username', this is your OLD password.
    oldPassword : string - If the type is 'password', this is your OLD username.
    newPassword : string - If the type is 'password', this is your NEW password.
```

Response structure:
```
Object
    error : bool - Has an error occurred? If successful, this should be false.
```

## ```/worker/packs```
This is an HTTP **get** request that lets you get all of the packs.

Response structure:   
```
Object
    error : bool - Has an error occurred?
    packs : object<string, object> - The packs, where the key is the pack name, and the value is the pack's information.
        Object
            price   : int - The price of the pack.
            color1  : string - The hex code of the first accent color of the pack.
            color2  : string - The hex code of the second accent color of the pack.
            image   : string - The pack's image.
            blooks  : array<string> - An array of the blooks in the pack.
            hidden  : bool - Is the pack hidden?
```

## ```/worker/rarities```
This is an HTTP **get** request that lets you get rarities of blooks.

Response structure:   
```
Object
    error    : bool - Has an error occurred?
    rarities : object<string, object> - The rarities, where the key is the rarity name, and the value is the rarity's information.
        Object
            color     : string - The rarity's theme color.
            animation : string - The rarity's animation (the animation that plays when getting a blook of this rarity)
            exp       : int - The experience points you get when opening a blook of this rarity.
            wait      : int - The wait time (in milliseconds) before the animation plays and the blook is revealed.
```

## ```/worker/blooks```
This is an HTTP **get** request that lets you get a list of all the blooks.

Response structure:
```
Object
    error    : bool - Has an error occurred?
    blooks   : object<string, object> - The entire list of all the blooks, where the key is the blook name, and the value is the blook's information.
        Object
            rarity   : string - The blook's assigned rarity.
            chance   : int - The blook's chance (1%-100%).
            price    : int - The selling price of the blook.
            image    : string - A relative URL of the blook's image (what the blook looks like).
            art      : string - A relative URL of the blook's art (the background of the blook that is visible in the sell menu).
```

## ```/worker/config```
This is an HTTP **get** request that lets you get the instance's config.

Response structure:
```
Object
    name         : string - The instance's name.
    version      : string - The instance's version.
    welcome      : string - The main text shown on the home screen (such as "First Private Server")
    description  : string - The description of the instance.
    pronunciation: string - The text description of the pronunciation.
    discord      : string - The instance's discord invite URL.
    plus         : string - The Blacket Plus subscription's price.
    rewards      : int - I don't exactly know what this does.
    exp          : object<string, int> - Information about experience.
    pages        : object<string, object> - Information about pages.
        Object
            link    : string - The link to the page.
            icon    : string - The FontAwesome icon name.
            isNews  : bool - If the page is the news page.
            location: string - The page's location on the navbar. For example, "bottom" or "left."
            perm    : string - The permission required to see the page.
```

## ```/worker/leaderboard```
This is an HTTP **get** request that lets you get the leaderboard.

Response structure:   
```
Object
    error    : bool - Has an error occurred?
    tokens   : array<object> - The token-based leaderboard. Commonly flooded with Debug pack spammers.
        Object
            username  : string - The user's username.
            role      : string - The user's role.
            tokens    : int - The user's amount of tokens.
            color     : string - The user's name color.
    exp      : array<object> - The exp-based leaderboard.
        Object
            username  : string - The user's username.
            role      : string - The user's role.
            exp       : int - The user's amount of experience points.
            color     : string - The user's name color.
            level     : int - The user's experience level.
```
