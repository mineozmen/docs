# Global utilities

```jsx
import global from "operations/global";
```

Application-wide utilities: configuration, HTTP, cookies, user preferences, navigation history, branches.

> Call methods on the object, as in `global.getConfig(...)`. Don't destructure.

***

## Configuration

#### `getConfig(variable)`

Reads a runtime variable. Application-level variables override environment variables, so a value configured for the current app wins.

```js
global.getConfig("API_URL");
global.getConfig("DEFAULT_LOCALE");
```

Use this rather than hardcoding URLs or locales in custom code. It keeps your component working across environments.

#### URL helpers

| Method                 | Returns                                                              |
| ---------------------- | -------------------------------------------------------------------- |
| `getAPIURL(path)`      | a full backend URL for `path`, respecting pass-through configuration |
| `getAPIBase()`         | the backend base URL                                                 |
| `getFileURL(path)`     | a full file-service URL                                              |
| `getAPIHeaders()`      | the standard JSON request headers                                    |
| `getAPIParams(params)` | `"?a=1&b=2"`, url-encoded (`""` when empty)                          |

***

## REST Calls

#### `fetchWrapper(url, opts, fn = "fetch", processCallback = null)`

**The way to call the backend from custom code.** On top of a plain `fetch` it:

* attaches the session token,
* sets `Accept-Language` from the user's selected locale,
* sets the branch header when a non-main branch is selected,
* transparently refreshes an expired token and retries the request once,
* redirects to the login page when the session cannot be recovered.

It returns the raw response, so you still call `.json()` or `.text()` yourself.

```js
const res = await global.fetchWrapper(
  global.getAPIURL("request/crud/order/export"),
  { method: "POST", headers: global.getAPIHeaders(), body: JSON.stringify(payload) }
);
if (res.ok) {
  const data = await res.json();
}
```

Passing `fn: "xhr"` routes through `XMLHttpRequest`, which enables the `processCallback` upload-progress callback (it receives the raw progress event with `loaded` / `total`). Use that for file uploads only; the response object it returns is simplified.

#### Downloads

| Method                     | Notes                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------------- |
| `downloadWrapper(url)`     | fetch and save; takes the filename from the response headers, falling back to the last URL segment |
| `download(blob, filename)` | save a blob you already have                                                                       |

> Plain `fetch()` inside custom code already retries transient connection failures, since that behaviour is installed application-wide. It does **not** add auth or locale headers, though, so prefer `fetchWrapper` for backend calls.

***

## Cookies, preferences and history

| Method                                             | Notes                                                  |
| -------------------------------------------------- | ------------------------------------------------------ |
| `setCookie(name, value, days)`                     | a falsy value expires the cookie                       |
| `getCookie(name)`                                  | `""` when absent                                       |
| `getPreferences()`                                 | the current user's stored preferences                  |
| `setPreference({ id, preference })`                | merge and persist one preference; returns the full set |
| `clearPreferences()`                               |                                                        |
| `getHistory()`                                     | recently visited pages                                 |
| `addPageToHistory({ url, title })`                 | keeps the most recent entries                          |
| `addDetailPageToHistory({ app, type, id, title })` | records a visited record under its list page           |
| `clearHistory()`                                   |                                                        |

For preferences **inside a component**, use the PreferenceProvider context instead of `getPreferences()`. The context re-renders when a preference changes, the getter does not.

***

## Branches

When branching is enabled, a record can exist both on main and on a branch and branched ids carry the branch as a prefix.

| Method                    | Notes                                                                                                    |
| ------------------------- | -------------------------------------------------------------------------------------------------------- |
| `getBranch()`             | the current branch, or `null` on main                                                                    |
| `setBranch(branch)`       |                                                                                                          |
| `applyBranch(branch, id)` | build a branched id                                                                                      |
| `splitBranch(id)`         | `{ branch, id }`; `branch` is `null` when unbranched                                                     |
| `pickBranchList(list)`    | collapse a mixed list to one entry per record, preferring the current branch's version, preserving order |

`pickBranchList` is what makes branched records appear as a single row. **If you build a custom list over a branch-enabled source, run the list through it**, otherwise users see duplicates.

***

## Misc

| Method                                                      | Notes                                                                                                     |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `generateUUID(withHyphens = false)`                         |                                                                                                           |
| `safeEncode(text)` / `safeDecode(base64)`                   |                                                                                                           |
| `formatLocale(locale)`                                      | `"enUS"` → `"en-US"`                                                                                      |
| `canAccess(userRoles, appConfig, screenConfig?)`            | role check; no configured roles means allowed                                                             |
| `getProp(obj, keys, default)` / `setProp(obj, keys, value)` | simple dotted access with no bracket escaping and no delete-on-null. For record data prefer `objectutils` |
| `getAggregateData(record, sendVersion = false)`             | strip a record down to the fields the backend accepts                                                     |
| `mapParameters(url, mode, body)`                            | move a body into the query string (`"query"`) or the path (`"url"`)                                       |
| `findUpTagByClass(el, className)`                           | walk up the DOM for an ancestor with a class                                                              |
| `printFrame(id)`                                            | focus an iframe and print it                                                                              |
| `coordinates`                                               | `{ lat, lon }` when geolocation is permitted                                                              |

#### Auth error codes

If you inspect an error `code` from an auth response, these are the ones you are most likely to see:

```
WRONG_CONFIGURATION 20001   SERVER_FAIL 20002    CLAIMS_FAIL 20003
INVALID_TYPE        20011   INVALID_DATA 20012   MISSING_DATA 20013
EXPIRED_TOKEN       20014   NOT_ALLOWED  20021
```

{% hint style="info" %}
**Full error code reference:** [Troubleshooting → Error codes](https://docs.rierino.com/troubleshooting/error-codes).
{% endhint %}
