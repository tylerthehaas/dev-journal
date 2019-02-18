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

