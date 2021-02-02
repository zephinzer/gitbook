# API Authentication

## Secret Token

On receiving API access, API user receives a secret token which is unique to their account. This token is used in requests to authenticate the user.

### Querystring-based

Secret token is added to the URL query parameters

```text
curl "https://api.com/account?token=${TOKEN}"
> {"id":12345,...}
```

Querystring-based usage of tokens is generally more insecure because of the possibility of systems logging the entire request URL including the token into a logging system which may not be secure

### Header-based

Secret token is added as a HTTP header

```text
curl -H "X-Auth: ${TOKEN}" https://api.com/account
> {"id":12345,...}
```

### Token-based

Secret token is sent in the request body

```text
curl -d '{"token":"eYabcd",...}' "https://api.com/account"
```

## Refresh Token

Similar in concept with Secret Token, but user receives a Refresh Token which is used to request for a Secret Token instead of being used directly.

This pattern improves security because if the Secret Token was leaked, it will not be valid forever to an attacker. The API consumer should handle the token renewal mechanism from a secure environment.

```text
curl -H "X-Refresh: ${REFRESH_TOKEN}" "https://api.com/token"
> {"token":"xyzabc","refresh_token":"defghi","expires_at":"2020-12-30T14:00:00+0800"...}
curl -H "X-Auth: ${TOKEN}" https://api.com/account
> {"id":12345,...}
```

## OAuth Token

OAuth is more commonly used in systems that enable an API consumer to perform actions on behalf of a user.

1. Direct user to an authentication page
2. Receive an authorization code after user authorizes API consumer
3. Exchange authorization code for an authentication token \(either a Secret Token or a Refresh Token\)
4. Use token to perform actions on behalf of another user

## SSL Certificate

1. On receiving API access, user receives an SSL certificate
2. User uses SSL certificate whenever they access the service

### When to use

1. User should only perform subsequent access to the service from the machine they used to register
2. User has technical expertise with managing their own SSL certificates
3. Inter-service communication \(aka mTLS in cloud-native systems\)

