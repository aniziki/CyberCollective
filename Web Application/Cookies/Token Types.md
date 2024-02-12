## What tokens are

For authentication and authorization, a token is a digital object that contains information about the identity of the principal making the request and what kind of access they are authorized for. In most authentication flows, the application—or a library used by the application—exchanges a credential for a token, which determines which resources the application is authorized to access.

## Types of tokens

Different types of tokens are used in different environments. The following token types are described on this page:

- [Access tokens](https://cloud.google.com/docs/authentication/token-types#access)
- [ID tokens](https://cloud.google.com/docs/authentication/token-types#id)
- [Self-signed JWTs](https://cloud.google.com/docs/authentication/token-types#self-signed)
- [Refresh tokens](https://cloud.google.com/docs/authentication/token-types#refresh)
- [Federated tokens](https://cloud.google.com/docs/authentication/token-types#federated)
- [Bearer tokens](https://cloud.google.com/docs/authentication/token-types#bearer)

This page does not discuss [API keys](https://cloud.google.com/docs/authentication/api-keys) or [Client IDs](https://cloud.google.com/docs/authentication#credentials), which are considered credentials.

## Access tokens

Access tokens are opaque tokens that conform to the [OAuth 2.0 framework](https://datatracker.ietf.org/doc/html/rfc6749#section-1.4). They contain authorization information, but not identity information. They are used to authenticate and provide authorization information to Google APIs.

If you use [Application Default Credentials (ADC)](https://cloud.google.com/docs/authentication/application-default-credentials) and the [Cloud Client Libraries or Google API Client Libraries](https://cloud.google.com/docs/authentication/client-libraries), you do not need to manage access tokens; the libraries automatically retrieve the credential, exchange it for an access token, and refresh the access token as needed.

### Access token contents

Access tokens are opaque tokens, which means that they are in a proprietary format; applications cannot inspect them. You can get the information from a valid (not expired or revoked) access token by using the Google OAuth 2.0 `tokeninfo` endpoint.

Replace ACCESS_TOKEN with the valid, unexpired access token.

curl "https://oauth2.googleapis.com/tokeninfo?access_token=ACCESS_TOKEN"

This command returns something similar to the following example:

{  
**"azp": "32553540559.apps.googleusercontent.com"**,  
"aud": "32553540559.apps.googleusercontent.com",  
"sub": "111260650121245072906",  
**"scope": "openid https://www.googleapis.com/auth/userinfo.email https://www.googleapis.com/auth/cloud-platform https://www.googleapis.com/auth/accounts.reauth"**,  
"exp": "1650056632", 
**"expires_in": "3488"**,  
"email": "user@example.com",  
"email_verified": "true"  
}  

The following table lists the most important fields to understand:

|Field|Description|
|---|---|
|`azp`|The project, email, or service account ID of the application that requested the token. This value is included only if `https://www.googleapis.com/auth/userinfo.email` is specified in the list of scopes.|
|`scope`|The OAuth scopes that have been added to this access token. For Google Cloud services, it is a best practice to use the `https://www.googleapis.com/auth/cloud-platform` scope, which includes all Google Cloud APIs, together with [Identity and Access Management (IAM)](https://cloud.google.com/iam/docs/overview), which provides fine-grained access control.|
|`expires_in`|The number of seconds until the token expires. For more information, see [Access token lifetime](https://cloud.google.com/docs/authentication/token-types#at-lifetime).|

### Access token lifetime

By default, access tokens are good for 1 hour (3,600 seconds). When the access token has expired, your token management code must get a new one.

If you need an access token with a longer or shorter lifetime, you can use the [`serviceAccounts.generateAccessToken` method](https://cloud.google.com/iam/docs/reference/credentials/rest/v1/projects.serviceAccounts/generateAccessToken) to create the token. This method enables you to choose the lifetime of the token, with a maximum lifetime of 12 hours.

If you want to extend the token lifetime beyond the default, you must create an organization policy that enables the [`iam.allowServiceAccountCredentialLifetimeExtension` constraint](https://cloud.google.com/resource-manager/docs/organization-policy/restricting-service-accounts#extend_oauth_ttl). You can't create access tokens with an extended lifetime for user credentials or external identities. For more information, see [Create a short-lived access token](https://cloud.google.com/iam/docs/creating-short-lived-service-account-credentials#sa-credentials-oauth).

## ID tokens

ID tokens are [JSON Web Tokens (JWTs)](https://jwt.io/introduction) that conform to the [OpenID Connect (OIDC) specification](https://openid.net/specs/openid-connect-core-1_0.html). They are composed of a set of key-value pairs called _claims_.

Unlike access tokens, which are opaque objects that cannot be inspected by the application, ID tokens are meant to be inspected and used by the application. Information from the token, such as Who signed the token or the identity for whom the ID token was issued, is available for use by the application.

For more information about Google’s OIDC implementation, see [OpenID Connect](https://developers.google.com/identity/protocols/oauth2/openid-connect). For best practices for working with JWTs, see [JSON Web Token Best Current Practices](https://datatracker.ietf.org/doc/html/rfc8725).

### ID token contents

You can inspect a valid (not expired or revoked) ID token by using the Google OAuth 2.0 `tokeninfo` endpoint.

Replace ID_TOKEN with the valid, unexpired ID token.

curl "https://oauth2.googleapis.com/tokeninfo?id_token=ID_TOKEN"

This command returns something similar to the following example:

{  
**"iss": "https://accounts.google.com"**,  
**"azp": "32555350559.apps.googleusercontent.com"**,  
**"aud": "32555350559.apps.googleusercontent.com"**,  
**"sub": "111260650121185072906"**,  
"hd": "google.com",  
"email": "user@example.com",  
"email_verified": "true",  
"at_hash": "_LLKKivfvfme9eoQ3WcMIg",  
**"iat": "1650053185"**,  
**"exp": "1650056785"**,  
"alg": "RS256",  
"kid": "f1338ca26835863f671403941738a7b49e740fc0",  
"typ": "JWT"  
}  

The following table describes required or commonly used ID token claims:

|Claim|Description|
|---|---|
|`iss`|The issuer, or signer, of the token. For Google-signed ID tokens, this value is `https://accounts.google.com`.|
|`azp`|Optional. Who the token was issued to.|
|`aud`|The audience of the token. The value of this claim must match the application or service that uses the token to authenticate the request. For more information, see [ID token `aud` claim](https://cloud.google.com/docs/authentication/token-types#id-aud).|
|`sub`|The subject: the ID that represents the principal making the request.|
|`iat`|[Unix epoch time](https://en.wikipedia.org/wiki/Unix_time) when the token was issued.|
|`exp`|[Unix epoch time](https://en.wikipedia.org/wiki/Unix_time) when the token expires.|

Other claims might be present, depending on the issuer and the application.

### ID token `aud` claim

The `aud` claim describes the service name this token was created to invoke. If a service receives an ID token, it must verify its integrity (signature), validity (is it expired) and if the `aud` claim matches the name it expects. If it does not match, the service should reject the token, because it could be a replay intended for another system.

Generally, when you [get an ID token](https://cloud.google.com/docs/authentication/get-id-token), you use the credentials provided by a service account, rather than user credentials. This is because the `aud` claim for ID tokens generated using user credentials is statically bound to the application the user used to authenticate. When you use a service account to acquire an ID token, you can specify a different value for the `aud` claim.

### ID token lifetime

ID tokens are valid for up to 1 hour (3,600 seconds). When an ID token expires, you must acquire a new one.

### ID token validation

When your service or application uses a Google service such as Cloud Run, Cloud Functions, or Identity-Aware Proxy, Google validates ID tokens for you; in these cases, the ID tokens must be signed by Google.

If you need to validate ID tokens within your application, you can do so, although this is an advanced workflow. For information, see [Validating an ID token](https://developers.google.com/identity/protocols/oauth2/openid-connect#validatinganidtoken).

## Self-signed JSON Web Tokens (JWTs)

Self-signed JWTs are required to [authenticate to APIs deployed with API Gateway](https://cloud.google.com/api-gateway/docs/authenticate-service-account). In addition, you can use self-signed JWTs to authenticate to some Google APIs without having to get an access token from the Authorization Server.

Creating self-signed JWTs is recommended if you are creating your own client libraries to access Google APIs, but is an advanced workflow. For more information about self-signed JWTs, see [Creating a self-signed JSON Web Token](https://cloud.google.com/iam/docs/creating-short-lived-service-account-credentials#sa-credentials-jwt). For best practices for working with JWTs, see [JSON Web Token Best Current Practices](https://datatracker.ietf.org/doc/html/rfc8725).

## Refresh tokens

By default, access tokens and ID tokens are valid for 1 hour. A refresh token is a special token that is used to obtain additional access tokens or ID tokens. When your application first authenticates, it receives an access token or ID token, as well as a refresh token. Later, if the application needs to access resources again, and the previously provided token has expired, it uses the refresh token to request a new token. Refresh tokens are used only for user authentication, such as for Cloud Identity or Google Workspace.

Refresh tokens don't have a set lifetime; [they can expire](https://developers.google.com/identity/protocols/oauth2#expiration), but otherwise they continue to be usable. For user access in Google Workspace or Cloud Identity premium edition, you can [configure the session length](https://support.google.com/a/answer/9368756) to ensure that a user must log in periodically to retain access to Google Cloud services.

If your application is creating and managing its own tokens, it also needs to manage refresh tokens. For more information, see the following links:

- [OAuth 2.0 for Server to Server Applications](https://developers.google.com/identity/protocols/oauth2/service-account)
- [OAuth 2.0 for Web Server Applications](https://developers.google.com/identity/protocols/oauth2/web-server).
- [OAuth 2.0 for Client-side Web Applications](https://developers.google.com/identity/protocols/oauth2/javascript-implicit-flow)
- [OAuth 2.0 for Mobile & Desktop Apps](https://developers.google.com/identity/protocols/oauth2/native-app)
- [OAuth 2.0 for TV and Limited-Input Device Applications](https://developers.google.com/identity/protocols/oauth2/limited-input-device)

## Federated tokens

Federated tokens are used as an intermediate step by [workload identity federation](https://cloud.google.com/iam/docs/workload-identity-federation). Federated tokens are returned by the [Security Token Service](https://cloud.google.com/iam/docs/reference/sts/rest) and cannot be used directly. They must be [exchanged for an access token](https://cloud.google.com/iam/docs/creating-short-lived-service-account-credentials) using service account impersonation.

## Bearer tokens

Bearer tokens are a general class of token that grants access to the party in possession of the token. Access tokens, ID tokens, and self-signed JWTs are all bearer tokens.

Using bearer tokens for authentication relies on the security provided by an encrypted protocol, such as `HTTPS`; if a bearer token is intercepted, it can be used by a bad actor to gain access.

If bearer tokens don’t provide sufficient security for your use case, consider adding another layer of encryption or using a mutual Transport Layer Security (mTLS) solution such as [BeyondCorp Enterprise](https://cloud.google.com/beyondcorp-enterprise/docs/securing-resources-with-certificate-based-access), which limits access to only authenticated users on a trusted device.



Source:
https://cloud.google.com/docs/authentication/token-types