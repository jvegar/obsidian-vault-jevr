[OAuth 2](https://oauth.net/2/) is an authorization framework that enables applications — such as Facebook, GitHub, and Digital Ocean — to obtain limited access to user accounts on an HTTP service.
It works by delegating user authentication to the service that hosts a user account and authorizing third-party applications to access that user account. OAuth 2 provides authorization flows for web and desktop applications, as well as mobile devices.

## OAuth Roles
- **Resource Owner**: The resource owner is the _user_ who authorizes an _application_ to access their account. The application’s access to the user’s account is limited to the scope of the authorization granted (e.g. read or write access)
- **Client**: The client is the _application_ that wants to access the _user’s_ account. Before it may do so, it must be authorized by the user, and the authorization must be validated by the API.
- **Resource Server**: The resource server hosts the protected user accounts.
- **Authorization Server**: The authorization server verifies the identity of the _user_ then issues access tokens to the _application_.

## Abstract Protocol Flow
![[mediafolder/abstract_flow.png]]
1. The _application_ requests authorization to access service resources from the _user_
2. If the _user_ authorized the request, the _application_ receives an authorization grant
3. The _application_ requests an access token from the _authorization server_ (API) by presenting authentication of its own identity, and the authorization grant
4. If the application identity is authenticated and the authorization grant is valid, the _authorization server_ (API) issues an access token to the application. Authorization is complete.
5. The _application_ requests the resource from the _resource server_ (API) and presents the access token for authentication
6. If the access token is valid, the _resource server_ (API) serves the resource to the _application_