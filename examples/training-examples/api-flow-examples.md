---
description: >-
  These flows can be viewed from Saga screen in Devops app, grouped under
  training domain
---

# API Flow Examples

All API workflows can be accessed via DevOps → Saga in the admin UI. In that screen, you’ll find the flows listed below under the “training” group.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Training Sagas</p></figcaption></figure>

## Saga: /train\_ping

This is the smallest end-to-end flow in the training domain. It wires a `Start` step straight to a `Success` step. It then echoes the incoming request payload back in the response. Use it as a smoke test for routing, authentication, and payload shape.

<figure><img src="../../.gitbook/assets/image (91).png" alt="" width="375"><figcaption><p>Ping Flow</p></figcaption></figure>

## Saga: /train\_hello

This flow adds a single transformation step to the basic ping pattern. It generates a deterministic output message, like `"Hello World"`, regardless of input. It is useful for learning how transform outputs become HTTP responses. It also helps confirm response mapping without touching any state.

<figure><img src="../../.gitbook/assets/image (92).png" alt="" width="302"><figcaption><p>Hello Flow</p></figcaption></figure>

## Saga: /train\_redirect

This flow demonstrates redirecting one endpoint to another saga. It reuses `train_hello` without defining its own step graph. That pattern keeps multiple endpoints consistent, while avoiding duplicate logic. It is also handy for deprecations and versioned routes.

<figure><img src="../../.gitbook/assets/image (93).png" alt="" width="563"><figcaption><p>Redirect Mapping</p></figcaption></figure>

## Saga: /train\_condition

This flow shows how branching works in sagas. A condition is evaluated and step links route execution to `Success` or `Fail`. It is a good template for validation or eligibility checks. Use the pattern when you need explicit “happy path” and “rejection path” responses.

<figure><img src="../../.gitbook/assets/image (94).png" alt="" width="375"><figcaption><p>Condition Flow</p></figcaption></figure>

## Saga: /train\_get

This flow demonstrates a read operation against a state manager. It calls a local read handler to fetch a `dummy` record by `id`. The response payload is built from the record returned by the handler. Use it to understand how saga inputs map to state reads.

<figure><img src="../../.gitbook/assets/image (95).png" alt="" width="305"><figcaption><p>Get Flow</p></figcaption></figure>

## Saga: /train\_set

This flow demonstrates a write operation against a state manager. It calls a local write handler to create or update a record in the `dummy` state manager. The request payload becomes the write input, with IDs and fields mapped by parameters. Use it as a baseline for POST/PUT style endpoints.

<figure><img src="../../.gitbook/assets/image (96).png" alt="" width="302"><figcaption><p>Set Flow</p></figcaption></figure>

## Saga: /train\_query

This flow demonstrates server-side filtering with a reusable query definition. It calls a local query handler and executes the `"Dummy Search"` query. The query filters records by fields such as `name` and returns a list. Use it to learn how query parameters travel from API input to the query engine.

<figure><img src="../../.gitbook/assets/image (97).png" alt="" width="305"><figcaption><p>Query Flow</p></figcaption></figure>

The query is accessible from the Query screen of the Configuration application. This is where the stored query text and parameters live. Updating it changes this endpoint’s behavior without editing the saga.

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption><p>Dummy Search Query</p></figcaption></figure>

## Saga: /train\_rest

This flow demonstrates an outbound HTTP call from a saga. It uses the REST handler together with a configured `System` definition. The saga builds a request, sends a GET call to the remote system, then forwards the response. Use it for simple integrations and for validating network access from runners.

<figure><img src="../../.gitbook/assets/image (170).png" alt="" width="375"><figcaption><p>Rest Flow</p></figcaption></figure>

This flow is also used in AI Agent example, and includes a simple input schema, which allows the AI agent to make calls to this flow in the correct data pattern. It also includes some optional instructions and mappings for the agent in AI tab.

## Saga: /train\_pattern

This flow builds on `train_rest` by parameterizing request and response mapping. It uses patterns in event metadata to inject values into the outbound request. It can also extract and reshape fields from the remote response into a clean API output. Use it when you need lightweight mapping without custom code.

<figure><img src="../../.gitbook/assets/image (171).png" alt=""><figcaption><p>Pattern Parameters</p></figcaption></figure>

## Saga: /train\_secret

This flow demonstrates basic crypto utilities in a saga. It encrypts an input value using a preconfigured key. It then decrypts the value back and returns it for verification. Use it to understand how secrets and encryption helpers fit into API flows.

<figure><img src="../../.gitbook/assets/image (65).png" alt="" width="366"><figcaption><p>Secret Flow</p></figcaption></figure>

## Saga: /train\_script

This flow executes a stored Groovy script from within the saga. The script reads parameters from the incoming payload. It then returns a computed payload, often echoing or reshaping inputs. Use it for fast prototyping when a transform step is not enough.

<figure><img src="../../.gitbook/assets/image (66).png" alt="" width="245"><figcaption><p>Script Flow</p></figcaption></figure>

The script itself is configured using the code editor accessible in Configuration application.

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption><p>Executed Groovy Script</p></figcaption></figure>

## Saga: /train\_template

This flow renders a text output using a stored Handlebars template. It passes inputs like `name` and `surname` into the template context. The rendered string is returned as the API response, which is useful for emails or HTML snippets. Use it to learn template parameter binding and output formatting.

<figure><img src="../../.gitbook/assets/image (70).png" alt="" width="250"><figcaption><p>Template Flow</p></figcaption></figure>

The template itself is configured using the code editor accessible in Configuration application.

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption><p>Rendered Handlebars Template</p></figcaption></figure>

## Saga: /train\_hft

This flow demonstrates dynamic Java compilation and execution at runtime. It loads a Java class definition from stored configuration. It compiles the class and runs it as an event handler during the saga. Use it for advanced experiments, where you need real code without redeploying.

<figure><img src="../../.gitbook/assets/image (72).png" alt="" width="248"><figcaption><p>HFT Flow</p></figcaption></figure>

The class itself is configured using the code editor accessible in Configuration application.

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption><p>Dynamic Java Class</p></figcaption></figure>

## Saga: /train\_rule

This flow evaluates a list of rules against the incoming payload. It returns the selected outcome value after rule matching. It is a good template for pricing, routing, or classification decisions. Use it to see how business rules can be changed without code changes.

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption><p>Simple Rule Flow</p></figcaption></figure>

The rule list for calculation is configured using the rule records accessible in Configuration application.

<figure><img src="../../.gitbook/assets/image (139).png" alt=""><figcaption><p>Rule Configuration</p></figcaption></figure>

## Saga: /train\_mock

This flow returns a fixed example response without calling any backend. It is intended for mocking endpoints during UI or integration development. You can keep contract tests moving while the real logic is not ready. Later you can swap the mock for real steps without changing the route.

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption><p>Mocking Configuration</p></figcaption></figure>

## Saga: /train\_for

This flow demonstrates looping over a list in the payload. It runs the same action for each item, such as rendering a template per entry. It typically produces an aggregated output list or concatenated result. Use it when you need per-item enrichment without writing code.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## Saga: /train\_parallel

This flow demonstrates fan-out execution. It triggers two independent branches at the same time. It then continues once both branches complete, typically combining their outputs. Use it to reduce latency when you call multiple systems.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## Saga: /train\_nested

This flow calls another saga from within a parent saga. It passes a payload into the nested saga and receives its output. This is the main reuse mechanism for shared business logic. Use it to keep complex flows modular and easier to test.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
