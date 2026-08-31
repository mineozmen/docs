---
description: >-
  Rierino includes several Python helpers for fine-tuning and complementing
  GenAI models
layout:
  width: wide
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# LLM Helpers

LLM helpers provided by Rierino can be deployed as stand-alone Jobs for long running processes, serviced through Python-Java bridge to embed into saga flows, or served over our Python API runner, exposing these capabilities as following standalone API endpoints:

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.ocr/OCRProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.xlsx/XLSXProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.docling/DoclingProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.docling/ChunkerProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.chunk/ChunkProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.anonymizer/AnonymizerProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.anonymizer/DeanonymizerProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="py-llm" path="/api/request/rierino_llm.tuner/TuneProcess" method="post" %}
[OpenAPI py-llm](https://4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/gitbook-x-prod-openapi/raw/cefbec32fae2fdbd6bf6c4763d6914ffa9caa77719990714d7ac2d3e8a9ba764.yaml?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20260831%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260831T162617Z&X-Amz-Expires=172800&X-Amz-Signature=b2574e19e5b7b96454f64708a12eab48fa874df6428fe819526ecaa21518ab66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}
