---
description: >-
  These flows can be viewed from Saga screen in Devops app, grouped under
  training domain
---

# API Flow Examples

## /train\_ping

This is the simplest API flow, connecting start step to a success step, returning request contents back as a response.

<figure><img src="../../.gitbook/assets/image (91).png" alt="" width="375"><figcaption><p>Ping Flow</p></figcaption></figure>

## /train\_hello

Another simple example, returning "Hello World" as a message in output with a transform step.

<figure><img src="../../.gitbook/assets/image (92).png" alt="" width="302"><figcaption><p>Hello Flow</p></figcaption></figure>

## /train\_redirect

This is a API flow redirect example, using train\_hello saga without providing any flow of its own.

<figure><img src="../../.gitbook/assets/image (93).png" alt="" width="563"><figcaption><p>Redirect Mapping</p></figcaption></figure>

## /train\_condition

This example includes conditional logic with step link configurations and both success & fail steps.

<figure><img src="../../.gitbook/assets/image (94).png" alt="" width="375"><figcaption><p>Condition Flow</p></figcaption></figure>

## /train\_get

Using a local read event handler, this example returns a record from the 'dummy' state manager by id.

<figure><img src="../../.gitbook/assets/image (95).png" alt="" width="305"><figcaption><p>Get Flow</p></figcaption></figure>

## /train\_set

Using a local write event handler, this example writes a given record to the 'dummy' state manager.

<figure><img src="../../.gitbook/assets/image (96).png" alt="" width="302"><figcaption><p>Set Flow</p></figcaption></figure>

## /train\_query

Using a local query event handler and a query named "Dummy Search", this example returns a list of records filtered by their name.

<figure><img src="../../.gitbook/assets/image (97).png" alt="" width="305"><figcaption><p>Query Flow</p></figcaption></figure>

The query is accessible from Query screen of Configuration application.

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption><p>Dummy Search Query</p></figcaption></figure>

## /train\_rest

Using a local rest event handler and a system definition, this example makes a GET request to a remote system.

<figure><img src="../../.gitbook/assets/image (98).png" alt="" width="297"><figcaption><p>Rest Flow</p></figcaption></figure>

## /train\_pattern

Similar to train\_rest, this example makes a GET request to a remote system, but transforming input and output using patterns in event metadata parameters.

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption><p>Pattern Parameters</p></figcaption></figure>

## /train\_secret

Example for encrypting a given value and decrypting it back with a pre-configured key.

<figure><img src="../../.gitbook/assets/image (65).png" alt="" width="366"><figcaption><p>Secret Flow</p></figcaption></figure>

## /train\_script

Executes a custom Groovy script which replicates the parameters provided in input as output.

<figure><img src="../../.gitbook/assets/image (66).png" alt="" width="245"><figcaption><p>Script Flow</p></figcaption></figure>

The script itself is configured using the code editor accessible in Configuration application.&#x20;

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption><p>Executed Groovy Script</p></figcaption></figure>

## /train\_template

Renders a Handlebars template for provided name, surname parameters.

<figure><img src="../../.gitbook/assets/image (70).png" alt="" width="250"><figcaption><p>Template Flow</p></figcaption></figure>

The template itself is configured using the code editor accessible in Configuration application.&#x20;

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption><p>Rendered Handlebars Template</p></figcaption></figure>

## /train\_hft

Allows real-time compilation and execution of Java event handlers for testing and development purposes.

<figure><img src="../../.gitbook/assets/image (72).png" alt="" width="248"><figcaption><p>HFT Flow</p></figcaption></figure>

The class itself is configured using the code editor accessible in Configuration application.&#x20;

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption><p>Dynamic Java Class</p></figcaption></figure>

## /train\_rule

Allows evaluation of a set of rules for the given input and outputs the decided value.

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption><p>Simple Rule Flow</p></figcaption></figure>

The rule list for calculation is configured using the rule records accessible in Train application.

<figure><img src="../../.gitbook/assets/image (139).png" alt=""><figcaption><p>Rule Configuration</p></figcaption></figure>

## /train\_mock

Returns an example response, mocking a currently unimplemented endpoint.

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption><p>Mocking Configuration</p></figcaption></figure>

## /train\_for

Renders templates in a for each loop for a given list.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## /train\_parallel

Executes two flows in parallel.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## /train\_nested

Calls another saga as a nested step.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
