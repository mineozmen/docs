---
description: >-
  A special API endpoint is used to pass custom variables to admin UI for
  various use cases
---

# Environment API

Defined by the "ENV\_URL" environment variable (defaulting to /GetAdminEnvironment), admin UI pulls global configuration parameters from backend services, which can be utilized in two main areas:

* Setting the global home page, by returning a landingTemplatePath field, including API endpoint (e.g. request/cms\_rpc/GlobalLandingPage) to be called for retrieving handlebars template to populate the page.
* Returning values, which are accessible by UI elements in patterns as \_meta.environment, such as customizing widget properties based on a backend calculation or configuration.
