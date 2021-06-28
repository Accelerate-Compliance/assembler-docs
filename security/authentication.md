# User Authentication security 
## Authentication servers and lists

Each Assembler application is linked to a specific user list within an Authentication server owned by an Assembler Organisation (see *Assembler Platform Architecture* for more information).

The user list maintains the list of users which have access to a specific application and provides authentication services, while managing third party identity providers.

## Authenticating with email and password

By default, each user list can enable email and password authentication for Assembler applications. This enables users to register, authorise email addresses, reset passwords and use that account to log in to any other Assembler Application that shares the same list.

Passwords are stored securely using bcrypt (OpenSSL implementation) and are never stored or transmitted in plain text. Salt round is determined in order to validate passwords over an acceptable timeframe of between 500ms and 1000ms.

Password policies can be configured at the user list level.

Applications can entirely remove the ability to register or login using email and password as required, favouring external IDPs.

## Multi-factor authentication

Multi-factor authentication (MFA) can be enabled at the application or user pool level. MFA strategies can optionally include:

1. External OTP Authenticator apps.
2. SMS-based MFA.
3. Email-based MFA (when specifically required).

## Authenticating with an external Identity Provider (IDP)

Assembler user lists support the ability to add and manage a variety of external IDPs to provide authentication for Assembler applications.

The following authentication mechanisms are currently supported, and can be enabled on a per application, user list, or Auth Server basis:

* SAML 2.0
* OAuth 2.0
* LDAP
* Google OAuth
* Microsoft Graph Authentication
* Facebook
* Twitter

## Session security

Assembler employs a variety of session security mechanisms:

### Secure and encrypted sessions

All cookies managed by Assembler are secure, HTTPS only, same-site cookies containing an encrypted session ID.

### Database store

Session storage is backed in the PostgreSQL datastore, allowing for remote logout and individual device management.

### Session expiry

Session inactivity and total time expiry limits can be configured at an application level depending on requirements.

### Session locking

Assembler utilises a 'session locking' technique, effectively limiting the session cookie to the IP address in which it was created. This limits effectively all session-jacking attacks.

This can be disabled if required as it may be incompatible with some corporate networks.

### 