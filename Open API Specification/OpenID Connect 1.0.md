# OpenID Connect 1.0 Specification
## What is OIDC (Open ID Connect)?
OIDC (**OpenID Connect**) is a simple identity layer *on top of* the **OAuth 2.0** protocol. It allows Clients to **verify the identity** of the End-User based on the **authentication** performed by an Authorization Server, as well as to **obtain basic profile information** about the End-User in an interoperable and REST-like manner.

In plain words, **OIDC = OpenID authentication + OAuth 2.0 + JWT**.

## Specification organization
There is a core specification with a set of optional additional support specification. Listing as below:

* **Core** – Defines the core OpenID Connect functionality: authentication built on top of OAuth 2.0 and the use of Claims to communicate information about the End-User
* Discovery – (Optional) Defines how Clients dynamically discover information about OpenID Providers
* Dynamic Registration – (Optional) Defines how clients dynamically register with OpenID Providers
* OAuth 2.0 Multiple Response Types – Defines several specific new OAuth 2.0 response types
* OAuth 2.0 Form Post Response Mode – (Optional) Defines how to return OAuth 2.0 Authorization Response parameters (including OpenID Connect Authentication Response parameters) using HTML form values that are auto-submitted by the User Agent using HTTP POST
* Session Management – (Optional) Defines how to manage OpenID Connect sessions, including postMessage-based logout and RP-initiated logout functionality
* Front-Channel Logout – (Optional) Defines a front-channel logout mechanism that does not use an OP iframe on RP pages
* Back-Channel Logout – (Optional) Defines a logout mechanism that uses direct back-channel communication between the OP and RPs being logged out
* OpenID Connect Federation – (Optional) Defines how sets of OPs and RPs can establish trust by utilizing a Federation Operator

<div align=center>
<img src=https://github.com/KOVERcjm/Technical_Notes/raw/master/Pictures/OIDC%20-%20specification%20diagram.png>
<br />
OIDC Specification Diagram
</div>

# OIDC code flow
```
+--------+                                   +--------+
|        |                                   |        |
|        |---------(1) AuthN Request-------->|        |
|        |                                   |        |
|        |  +--------+                       |        |
|        |  |        |                       |        |
|        |  |  End-  |<--(2) AuthN & AuthZ-->|        |
|        |  |  User  |                       |        |
|   RP   |  |        |                       |   OP   |
|        |  +--------+                       |        |
|        |                                   |        |
|        |<--------(3) AuthN Response--------|        |
|        |                                   |        |
|        |---------(4) UserInfo Request----->|        |
|        |                                   |        |
|        |<--------(5) UserInfo Response-----|        |
|        |                                   |        |
+--------+                                   +--------+
```

In the diagram, ‘AuthN’ stands for ‘Authentication’ and ‘AuthZ’ stands for ‘Authorization’. A more detailed diagram is shown as followed.
<div align=center>
<img src=https://github.com/KOVERcjm/Technical_Notes/raw/master/Pictures/OIDC%20-%20request%20detail.png>
<br />
</div>

# Major improvement
The major improvement of OIDC are *ID token*, *user-info endpoint* and standard set of *scopes*.

## ID token
The ID Token is a security token that contains Claims about the **authentication** of an End-User *by an Authorization Server* when using a Client, and potentially other requested Claims. The ID Token is represented as a JSON Web Token (**JWT**).
The following Claims are used within the ID Token:

* **iss** - REQUIRED. Issuer Identifier for the Issuer of the response. The iss value is a case-sensitive URL using the https scheme with no query or fragment components.
* **sub** - REQUIRED. Subject Identifier. Locally unique and never reassigned identifier within the Issuer for the End-User.
* **aud** - REQUIRED. Audience(s) that this ID Token is intended for. It MUST contain the OAuth 2.0 client_id of the Relying Party as an audience value. It MAY also contain identifiers for other audiences. 
* **exp** - REQUIRED. Expiration time on or after which the ID Token MUST NOT be accepted for processing. 
* **iat** - REQUIRED. Time at which the JWT was issued. 
* ‘auth_time’ - Time when the End-User authentication occurred. When a max_age request is made then this Claim is REQUIRED; otherwise, its inclusion is OPTIONAL.
* ‘nonce’ - OPTIONAL. String value used to associate a Client session with an ID Token, and to mitigate replay attacks. The value is passed through unmodified from the Authentication Request to the ID Token.
* ‘at_hash’ - OPTIONAL. Access Token hash value. This is OPTIONAL when the ID Token is issued from the Token Endpoint, which is the case for this subset of OpenID Connect; nonetheless, an at_hash Claim MAY be present. 
* ‘acr’ - OPTIONAL. Authentication Context Class Reference. 
* ‘amr’ - OPTIONAL. Authentication Methods References. JSON array of strings that are identifiers for authentication methods used in the authentication. For instance, values might indicate that both password and OTP authentication methods were used. 
* ‘azp’ - OPTIONAL. Authorized party - the party to which the ID Token was issued. If present, it MUST contain the OAuth 2.0 Client ID of this party. This Claim is only needed when the ID Token has a single audience value and that audience is different than the authorized party. It MAY be included even when the authorized party is the same as the sole audience.

ID Tokens MAY contain other Claims. Any Claims used that are not understood MUST be ignored.

The following is a non-normative example of the set of Claims (the JWT Claims Set) base64url-decoded from an ID Token:
``` JSON
  {
   "iss": "https://server.example.com",
   "sub": "24400320",
   "aud": "s6BhdRkqt3",
   "exp": 1311281970,
   "iat": 1311280970
  }
```

## UserInfo Endpoint
The UserInfo Endpoint is an OAuth 2.0 *Protected* Resource that returns **Claims** about the authenticated End-User. The returned Claims are represented by a **JSON** object that contains a collection of name and value pairs for the Claims. Communication with the UserInfo Endpoint MUST utilize **TLS**.

The UserInfo Endpoint is an OAuth 2.0 Protected Resource that complies with the *OAuth 2.0 Bearer Token* Usage specification. The request SHOULD use the HTTP **GET** method and the **Access Token** SHOULD be sent using the Authorization header field.

## Standard set of scopes
OpenID Connect Clients use **scope** values as defined in OAuth 2.0 to specify what access privileges are being requested for Access Tokens. The scopes associated with Access Tokens determine what resources will be available when they are used to access OAuth 2.0 protected endpoints. For OpenID Connect, scopes can be used to request that specific sets of information be made available as Claim Values.

To distinguish with OAuth 2.0 request, OpenID Connect requests MUST contain the **openid** scope value. OPTIONAL scope values of profile, email, address, phone and offline_access are also defined.

# *Reference*
* [OpenID Connect Official](https://openid.net/connect/)
* [OpenID Basic Guide](https://openid.net/specs/openid-connect-basic-1_0.html)
* [[认证 & 授权] 4. OIDC（OpenId Connect）身份认证（核心部分）](https://www.cnblogs.com/linianhui/p/openid-connect-core.html)
* [瞭解 OpenID Connect 認證機制](https://medium.com/@petertc/openid-connect-a27e0a3cc2ae)