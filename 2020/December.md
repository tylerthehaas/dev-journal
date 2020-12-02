# December 2nd, 2020

## Platform Routing

Ops maintains a mapping of route urls to app urls so that when a request is made to the domain `app.pluralsight.com` that request can be forwarded to the right place to handle the request.

For Example:

```json
{
  "app.pluralsight.com/priorities": "app.priorities.com"
}
```

client request -> `app.pluralsight.com/priorities` -> priorities express server -> priorities SPA. 

`#routing`
