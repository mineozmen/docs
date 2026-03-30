---
description: >-
  All AI agents configured on Rierino are automatically accessible as APIs and
  can be incorporated into any app for visual interaction
---

# AI Agent APIs

All runners which include GenAI base runner and have GenAI models assigned to them can service the following common APIs for describing and interacting with agents.

{% openapi-operation spec="rierino-api" path="/api/request/{channel}/GetAIAgent" method="get" %}
[OpenAPI rierino-api](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/79622f5efd9b4bde6bf274b61fa6ced03715560d73e97fd575bf633802237b5c.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260330%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260330T140846Z&X-Amz-Expires=172800&X-Amz-Signature=ed18de3e76a507d49ec8e68ac58fcfbf6992b6b28ac45758b1291473d6c796ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="rierino-api" path="/api/request/{channel}/CallAIAgent" method="post" %}
[OpenAPI rierino-api](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/79622f5efd9b4bde6bf274b61fa6ced03715560d73e97fd575bf633802237b5c.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260330%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260330T140846Z&X-Amz-Expires=172800&X-Amz-Signature=ed18de3e76a507d49ec8e68ac58fcfbf6992b6b28ac45758b1291473d6c796ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="rierino-api" path="/api/request/{channel}/CallAIPanel" method="post" %}
[OpenAPI rierino-api](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/79622f5efd9b4bde6bf274b61fa6ced03715560d73e97fd575bf633802237b5c.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260330%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260330T140846Z&X-Amz-Expires=172800&X-Amz-Signature=ed18de3e76a507d49ec8e68ac58fcfbf6992b6b28ac45758b1291473d6c796ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}
