# April 5th, 2019

## FB migration to /users/:id

### updating avatar URL

Currently backend is still working on an endpoint to store avatar images for a user.

until that is finished we will need to store avatar images in firebase and then do a PUT to railsapi to update the avatarUrl.
