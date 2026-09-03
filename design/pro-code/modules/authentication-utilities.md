---
description: Authentication utilities provide access to user & login functions
---

# Authentication utilities

```jsx
import auth from "operations/auth";
```

The current user, their roles and the login flows. This is browser-only: all of it depends on the active session in the browser.

> Call methods on the object, as in `auth.getUser()`. Don't destructure.

***

## Who is signed in

#### `getUser()`

Returns the signed-in user's claims as an object, or `null` when there is no session.

```jsx
const user = auth.getUser();
// user.sub, user.name, user.email, user.roles, … whatever your identity
// provider issues
```

#### `getUserRoles()`

The user's roles as an array, normalised whether the token carries them as an array or as a comma-separated string. Returns `[]` when there are none.

```jsx
if (auth.getUserRoles().includes("admin")) {
  // show the admin action
}
```

For screen-level access checks, combine it with `global.canAccess`:

```jsx
import global from "operations/global";
const allowed = global.canAccess(auth.getUserRoles(), props.appConfig, props.screenConfig);
```

#### `hasUser()`

`true` when a session token exists. Note this is not a validity check; an expired token still counts.

***

## Login flows

You only need these inside a **login template** component. Everywhere else the session is already established and expired tokens are refreshed automatically behind `global.fetchWrapper`.

#### `login(stage, stageData, keepLoggedIn, captchaValue)`

Multi-stage login.

* Stage `"start"`: `stageData` is `{ username, password }`.
* Any later stage (MFA code, password reset, …): `stageData` carries that step's fields.

Returns `{ status: 1, data }` on success, with the session already established, or `{ status: -1, code, message, data }` on failure. The `code` matches the auth error codes listed in global.

#### `logoff()`

Ends the session and clears local history and preferences. Returns `{ status: 1 }`, or `{ status: -1, code, message }` if the server call failed. The local session is cleared either way.

#### `refresh()`

Renews the session. You should not need to call this: it happens automatically when a request hits an expired token.

#### `captcha(captchaValue)`

Verifies a reCAPTCHA response. Returns `{ status: 1 }` or `{ status: -1 }`.

***

## Token management

`setTokens(data)` and `clearTokens()` exist for the login flow. Outside of a login template, leave them alone: clearing tokens mid-session logs the user out.
