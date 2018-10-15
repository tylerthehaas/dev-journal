# What is a JWT?

a standard format for providing a series of claims in a compact way while including a verifiable signature of the authenticity of those claims.

standardizes certain claims that are a part of common operations.

# What problem does it solve?

When combined with JSON Web Encryption (JWE) and JSON Web Signatures (JWS) it can provide secure solutions to many problems.

Very useful in the context of microservices. In a microservices architecture you have many different services communicating in many different directions so that each microservice can be deployed and managed independently significantly lowering dependencies any one microservice may have. If you have a client that communicates with multiple microservices in your typical application you would have a user login and that would cause a authentication request to be fired off to a service. upon successful authentication a session would be opened and a session token would be returned to the client. Now the service that created the session token knows they are a logged in user however none of the other services do. So each time a new request is initiated by the client to any microservice it will send that token and the receiving microservice will need to verify that the token is valid. This creates a single point of failure at the authentication service for anything done within the entire system. JWT's solve this by providing data and a signature along with each request removing the dependency that all other services have on the authentication service.
