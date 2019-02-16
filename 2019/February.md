# February 16th, 2019

## aws authorization with multipart payload

* [signature calculations](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-header-based-auth.html)
* [ Using the Authorization Header](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-auth-using-authorization-header.html#sigv4-auth-header-overview)
  - POST requests dont use `Authorization` request headers.
* [POST Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPOST.html)
  > Parameters that are passed to PUT via HTTP Headers are instead passed as form fields to POST in the multipart/form-data encoded message body. You must have WRITE access on a bucket to add an object to it. Amazon S3 never stores partial objects: if you receive a successful response, you can be confident the entire object was stored.

