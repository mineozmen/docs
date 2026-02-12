---
description: >-
  All AI agents configured on Rierino are automatically accessible as APIs and
  can be incorporated into any app for visual interaction
---

# AI Agent APIs

All runners which include GenAI base runner and have GenAI models assigned to them can service the following common APIs for describing and interacting with agents.

{% openapi-operation spec="rierino-api" path="/api/request/{channel}/GetAIAgent" method="get" %}
[OpenAPI rierino-api](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/1ac2442116ef7126bdea09264fc54f3979ce5612179bd51be363cdfc28929bd9.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260212%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260212T175616Z&X-Amz-Expires=172800&X-Amz-Signature=14070786424a81d19a71d0c4cb36b2f67b32d6d092adaefe892ecf67d9f76950&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="rierino-api" path="/api/request/{channel}/CallAIAgent" method="post" %}
[OpenAPI rierino-api](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/1ac2442116ef7126bdea09264fc54f3979ce5612179bd51be363cdfc28929bd9.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260212%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260212T175616Z&X-Amz-Expires=172800&X-Amz-Signature=14070786424a81d19a71d0c4cb36b2f67b32d6d092adaefe892ecf67d9f76950&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}
