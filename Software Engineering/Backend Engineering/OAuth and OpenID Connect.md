# Explanation

## Problem (Delegated Authorization)
- Before the advent of OAuth 2.0, granting third-party applications access to user data on another service often required users to share their credentials directly with the application.
- This practice posed significant security risks, as it exposed user credentials to potentially untrustworthy applications.
- OAuth 2.0 was developed to solve this "delegated authorization problem" by allowing users to grant limited access to their data without sharing their credentials.
- Illustration:
	- 1: ![[Delegated Authorization 1.png]]
	- 2: ![[Delegated Authorization 2.png]]

### The Yelp Example
- In the early days of the internet, companies like Yelp attempted to solve this problem by asking users for their email login credentials.
- This practice was highly insecure and highlighted the need for a more robust solution.
- Don't do this: ![[Are you friends already on Yelp?.png]]

##  Terminology
- **Resource Owner:** The user who owns the protected data and grants permission to access it.
- **Client:** The application requesting access to protected resources on behalf of the user.
- **Authorization Server:** The system responsible for user authentication, obtaining consent, and issuing access tokens and ID tokens.
- **Resource Server:** The system hosting the protected resources, responsible for verifying access tokens and granting access to data.
- **Access Token:** A credential provided to the client after successful authorization, allowing access to protected resources on the resource server within the defined scope.
- **Authorization Code:** A temporary code issued by the authorization server after user consent, which is exchanged for an access token on the back channel.
- **Scopes:** Defines the specific permissions an application requests, controlling the level of access granted to the client.

## OAuth 2.0 Authorization Code Flow
### Explanation
1. **Requesting Authorization:** The client initiates the flow by directing the user's browser to the authorization server. The request includes the client ID, requested scopes, and the redirect URI.
2. **User Authentication and Consent:** The authorization server authenticates the user and presents a consent screen detailing the scopes requested by the client. The user can then choose to grant or deny access.
3. **Issuing the Authorization Code:** Upon user consent, the authorization server issues an authorization code to the client. This code is transmitted back to the client via the redirect URI specified in the initial request.
4. **Exchanging the Code for an Access Token:** The client exchanges the authorization code for an access token through a secure back-channel communication with the authorization server. This exchange involves the client ID, client secret, and the authorization code.
5. **Accessing Protected Resources:** With the obtained access token, the client can now access the protected resources on the resource server. The access token is presented in the authorization header of the request.
### Illustration
![[OAuth 2.0 authorization code flow.png]]

## Problems with OAuth 2.0 for authentication
- No standard way to get the user's information
- Every implementation is a little different
- No common set of scopes

## OpenID Connect
### Explanation
- *OpenID Connect*: an authentication layer built on top of OAuth 2.0, standardizing the process of obtaining user information and enabling single sign-on.
- OpenID Connect adds:
	- **ID Token:** A JSON Web Token (JWT) containing information about the authenticated user, including user ID, email address, and token expiration time.
		- It allows applications to verify user identity and access basic profile information.
	- **User Info Endpoint:** A dedicated endpoint that allows clients to retrieve additional user information using the access token.
	- **Standard set of scopes**.
	- **Standardized implementation**.
### Illustration
![[OpenID Connect authorization code flow.png]]

# Sources
- [OktaDev - OAuth 2.0 and OpenID Connect (in plain English)](https://www.youtube.com/watch?v=996OiexHze0)
