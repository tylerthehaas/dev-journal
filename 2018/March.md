# March 5th, 2018

to build a local docker image:

`docker build . -t [appname]`

to run a local docker image:

`docker run -itp 8080:8080 [appname]`

# March 14th, 2018

## issue caused by reduce
Calling reduce() on an empty array without an initial value is an error

## vision router not forwarding requests

When I am logging out traffic-perf-dev I get a log like this

```
2018-03-14T17:51:16Z traffic-perf-dev akkeris/router: host=traffic-perf-dev.alamoapp.octanner.io fwd=172.25.16.237%10 method=GET path=/favicon.ico request_id=8095d897f17904222a2cd2b6d3a0c817 status=200 service=9 connect=8 total=17 bytes=0
```

However I do not get anything in the logs for yrbkconfui-ca-dev-us. I think the cause of this is that it is forwarding it on to some localhost route.

# March 15th, 2018

## vision router not forwarding requests

solved by creating an entry in sites and routes within alamo via the following command.

```
aka routes:create -s vision-dev.appreciatehub.com -a traffic-perf-dev /ui/yearbook/config /ui/yearbook/config
```

## Testing yearbook config in dev

bank of america was a good test client

STP: 0001252855

# March 19th, 2018

## Grep usage

```
grep 'word' filename
grep 'word' file1 file2 file3
grep 'string1 string2'  filename
cat otherfile | grep 'something'
command | grep 'something'
command option1 | grep 'data'
grep --color 'data' fileName
```

## jQuery Templates

[An Introduction to jQuery Templates](http://stephenwalther.com/archive/2010/11/30/an-introduction-to-jquery-templates)

## [how to detect a mobile device in JavaScript](https://stackoverflow.com/questions/3514784/what-is-the-best-way-to-detect-a-mobile-device-in-jquery)

## [how to debug from an iphone](https://stackoverflow.com/questions/7242997/is-there-a-way-to-debug-javascript-in-the-iphone-ios-safari-browser)

## [How to debug from an iphone's web browser](https://stackoverflow.com/questions/7242997/is-there-a-way-to-debug-javascript-in-the-iphone-ios-safari-browser)

## Cannot hit my local dev server from iphone

tried following the advice on this stackoverflow [How do you access a website running on localhost from iPhone browser](https://stackoverflow.com/questions/3132105/how-do-you-access-a-website-running-on-localhost-from-iphone-browser/41857012#41857012) through running `ifconfig | grep "inet " | grep -v 127.0.0.1`. I was able to determine my ip address is either `172.25.17.178` or `172.25.31.255`. However when I visited `http://172.25.17.178:8443/a` or `http://172.25.17.178:8443/a`. I am told that `Safari could not open the page because the server stopped responding`.

# March 20th, 2018

## Cannot hit my local dev server from iphone

was referred to [ngrok](https://ngrok.com/docs). It seems to work however it returns a `502 - Bad Gateway` response. As far as I can tell ngrok has some pretty simple functionality of setting up a public url that will simply direct traffic to localhost. Tested that ngrok works using the ghoster application and it seems to route traffic from the iphone to my localhost correctly.
 At this point I am thinking that milestones doesn't allow traffic to be routed to it from a public URL. 

There is nothing blocking routing traffic to milestones from a public URL. I was able to get around this by using Port 8080 however this will not allow me to login because of how auth cookies are set in yearbook.

Turns out I didn't need to worry about doing this. If I use the chrome emulator in dev tools it shows a mobile user agent.

## empty div creating extra space

Tried to wrap data in if statements but that did not work because the data is an array of objects. I wrapped them all in if statements to protect empty data from rendering but that didn't change anything. I am thinking that I need to find a property that is different between products and filler images. 

I was wrong here as well. It was trying to get an object at index 4 however the data will only ever contain up to 3 indeces because of the slice that is being done.

# March 21st, 2018

## How to persist changes made in devtools

You can persist changes made in the sources directory and even have those changes save to your src files

[Set Up Persistence with DevTools Workspaces](https://developers.google.com/web/tools/setup/setup-workflow)

Also:

[Local Overrides](https://developers.google.com/web/updates/2018/01/devtools#overrides)


## [Add custom headers to 'request'](https://stackoverflow.com/questions/38332492/add-custom-headers-to-request)

## writing custom middleware in express.js

[express docs for app.use](http://expressjs.com/en/4x/api.html#app.use)

takes a callback with this signature: `req -> res -> next`.
When you are done just call `next()` to move on to the next middleware.

# March 27nd, 2018

`oc tanner login: Eva 05575193134396`

# March 28th, 2018

## not block brochure not using local version of yearbook-assets

After following all directions in readme of yearbook-assets I was still getting the assets from the cdn. One thing that is left out of the readme is that you have to recompile blockbrochure after changing blockbrochure.properties in order to change to using your local version of yearbook-assets.

# March 29th, 2018

## Get commit log for a certain line

using -L flag on git blame does this for you.

ex. `git blame -L 150,+11 -- git-web--browse.sh`

## [Info on proxies](https://ponyfoo.com/articles/es6-proxies-in-depth)


