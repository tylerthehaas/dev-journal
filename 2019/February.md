# February 13th, 2019

## deleting vs. archiving in align

Firebase doesn't provide a way to do a soft delete of data as referenced [here](https://firebase.google.com/docs/database/web/read-and-write#updating_or_deleting_data). So in order to do this we do an update to the referenced row with an archive flag using the `firebase#update` method and then filter out archived data client side.

## test if HTMLElement matches selector with Element.matches

If you want to find out if an html element matches a given css selector [Element.matches](https://developer.mozilla.org/en-US/docs/Web/API/Element/matches) will do this for you!

# February 16th, 2019

## aws authorization with multipart payload

* [signature calculations](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-header-based-auth.html)
* [ Using the Authorization Header](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-auth-using-authorization-header.html#sigv4-auth-header-overview)
  - POST requests dont use `Authorization` request headers.
  > Except for POST requests and requests that are signed by using query parameters, all Amazon S3 bucket operations and object operations use the Authorization request header to provide authentication information.
* [POST Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPOST.html)
  > Parameters that are passed to PUT via HTTP Headers are instead passed as form fields to POST in the multipart/form-data encoded message body. You must have WRITE access on a bucket to add an object to it. Amazon S3 never stores partial objects: if you receive a successful response, you can be confident the entire object was stored.
  - Because fetch automatically sets the header `Content-Type: multipart/form-data; boundary=<identifier>` when it detects that form data is passed in the request body. We should be able to pass security protocol with the following:
 ```javascript
 const formData = new FormData();
formData.append('AWSAccessKeyId', string<AccessKey>);
formData.append('file', File|string<TextContent>);
formData.append('key', path/to/destination/directory/in/s3/${FileName});
```
* [Object Key and Metadata](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html)

# February 23rd, 2019

## AWS Lambda functions

### Tell AWS about your functions

create a serverless.yml file that will hold the descriptions of all your lambda functions. It will have the following structure:

```
functions:
  <function-name>:
    description: <description text>
    handler: <path/to/function-file.exportedvariablename>
    memorySize: <memorySize>
    events:
      - http:
          path: <route>
          method: <get|put|post|patch|delete>
          cors: <boolean>
          authorizer: <authorizationProvider>
    environment:
```

### create your function

function has the following signature:

```
exports.exportedvariablename = async (event, context, callback) => {
  ...body of function
  callback();
};
```

* event has the structure [found here](https://stackoverflow.com/questions/41648467/getting-json-body-in-aws-lambda-via-api-gateway#answer-41656022).
* context provides meta data about the functions invocation, function and execution environment. [more here](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-context.html)
* callback will be what is returned to the caller of the function.

See [Getting Started](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.NodeJs.03.html#GettingStarted.NodeJs.03.03) for more info.

# February 28th, 2019

## search instagram hashtags for user and media data

1. call the `me/accounts?fields=instagram_business_account,name` edge to get list of accounts for logged in user.
  - [getting started](https://developers.facebook.com/docs/instagram-api/getting-started#test)
  - response will look like
  ```javascript
  {
  "data": [
      {
        "instagram_business_account": {
          "id": "17841403366879346"
        },
        "name": "Bragify",
        "id": "1504513843188856"
      }
    ],
    "paging": {
      "cursors": {
        "before": "MTUwNDUxMzg0MzE4ODg1NgZDZD",
        "after": "MTUwNDUxMzg0MzE4ODg1NgZDZD"
      }
    }
  }
  ```

2. call the `ig_hashtag_search?q=${hashtag}&user_id=${data.instagram_business_account.id}` to get a list of posts for a given hashtag
  - [IG Hashtag Search](https://developers.facebook.com/docs/instagram-api/reference/ig-hashtag-search)
  - response will look like
  ```javascript
  {
    "data": [
      {
        "id": "18040072057042872"
      }
    ]
  }
  ```

3. call the `/${hashtag_id}/recent_media?fields=${fieldList}&user_id=${data.instagram_business_account.id}` edge to get a list of instagram posts.
  - [Recent Media](https://developers.facebook.com/docs/instagram-api/reference/hashtag/recent-media)
  - response will look like
  ```javascript
  {
    "data": [
      {
        "id": "18041584465057239"
      },
    ],
  }
  ```

  - possible fields are
    * caption
    * comments_count
    * id
    * like_count
    * media_type
    * media_url
    * permalink

3. Use `/${media.id}` edge to get detailed info for every media object returned in `recent_media` edge.
  - [Media](https://developers.facebook.com/docs/instagram-api/reference/media)
  - response structure
    ```javascript
    {
      "id": "17895695668004550",
      "media_type": "IMAGE",
      "media_url": "https://fb-s-b-a.akamaihd.net/h-ak-fbx/t51.2885-9/21227247_1640962412602631_3222510491855224832_n.jpg?_nc_log=1",
      "owner": {
        "id": "17841405822304914"
      },
      "timestamp": "2017-08-31T18:10:00+0000"
    }
    ```
  - available fields
    * caption* (excludes carousel children)
    * children* (carousel albums only)
    * comments (excludes carousel children, replies to comments, and the caption)
    * comments_count* (excludes carousel children, includes replies, includes the caption)
    * id*
    * ig_id
    * is_comment_enabled (excludes carousel children)
    * like_count* (excludes carousel children, includes replies)
    * media_type*
    * media_url*
    * owner (only returned if the User making the query also owns the media object, otherwise the username field will be included)
    * permalink*
    * shortcode
    * thumbnail_url (only available on videos)
    * timestamp*
    * username*

4. use `/${owner.id}` edge to get details on the user who posted the media in step 3.
  - [User](https://developers.facebook.com/docs/instagram-api/reference/user)
  - sample response
    ```javascript
    {
      "biography": "Dino data crunching app",
      "id": "17841405822304914",
      "username": "metricsaurus",
      "website": "http://www.metricsaurus.com/"
    }
    ```

  - available fields
    * biography*
    * id*
    * ig_id
    * followers_count*
    * follows_count
    * media_count*
    * name
    * profile_picture_url
    * username*
    * website*
