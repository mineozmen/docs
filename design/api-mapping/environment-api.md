---
description: >-
  A special API endpoint is used to pass custom variables to admin UI for
  various use cases
---

# Environment API

Admin UI can load a global “environment” payload from the backend. This payload is meant for small, UI-focused configuration values.

The endpoint is defined by the `ENV_URL` environment variable. It defaults to `/GetAdminEnvironment`.

On page load (and when the UI decides to refresh), Admin UI calls this endpoint. It then exposes the returned values to screens and widgets.

## Home Page Configuration

If the environment API returns the fields below, Admin UI can change the landing behavior. This is useful for branding, custom dashboards, and AI entry points.

* **landingRedirect**: For redirecting global home page visits to another admin UI page (e.g. /app/custom)
* **landingTemplatePath**: For pulling template for the global home page (e.g. request/cms\_rpc/GlobalLandingPage)
* **aiTemplatePath**: For pulling template for the AI agent home page (e.g. request/cms\_rpc/AIAgentsPage)

{% hint style="info" %}
Template paths are gateway API paths. Use the same format you use elsewhere in Admin UI, like `request/...`.
{% endhint %}

## UI Widget Customizations

Any other returned values are available to UI elements under `_meta.environment`. Use this for small feature flags and UI tweaks driven by the backend.

### Response example

The environment API should return a JSON object. Keep it flat and small when possible.

```json
{
  "landingRedirect": "/app/custom",
  "landingTemplatePath": "request/cms_rpc/GlobalLandingPage",
  "aiTemplatePath": "request/cms_rpc/AIAgentsPage",
  "features": {
    "enableAdvancedSearch": true
  },
  "ui": {
    "defaultRegion": "EU"
  }
}
```

This becomes accessible as:

* `_meta.environment.landingRedirect`
* `_meta.environment.features.enableAdvancedSearch`
* `_meta.environment.ui.defaultRegion`

### Good practices

* Do not return secrets. This payload is visible to any user with UI access.
* Prefer stable keys. Changing key names breaks widgets and templates.
* Keep it fast. It is called during UI initialization.
* Use it for UI configuration only. Do not treat it as a general data API.

### Troubleshooting

* **Nothing shows up in `_meta.environment`:** Verify `ENV_URL` is set and reachable from the UI.
* **Landing template does not render:** Check that `landingTemplatePath` points to a working gateway endpoint.
