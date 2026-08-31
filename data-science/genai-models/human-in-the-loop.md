---
description: >-
  Configure human-in-the-loop controls for Rierino AI agents, including
  confirmations, enforced approvals, drafts, and task handoffs.
---

# Human-in-the-Loop

Human-in-the-loop (HITL) controls keep people accountable for AI agent actions. They help prevent unintended changes when an agent has incomplete context or a request carries business risk.

Rierino supports HITL at several points in an agent workflow. Choose a pattern based on the action's risk, the required reviewer, and whether the conversation can wait.

Use HITL alongside [ai-guardrails.md](ai-guardrails.md "mention"). Guardrails inspect content and tool results. HITL controls whether an action can proceed.

### Choose the right HITL pattern

Use **soft controls** when the user must clarify intent. The agent can continue only after a clear answer.

Use **hard controls** when an action must never occur outside policy even with user approval. The tool saga rejects invalid or unauthorized calls.

Use **hard approvals** when a person must explicitly authorize a specific action. The agent pauses before the tool executes.

Use **draft actions** when a reviewer needs to inspect a generated record offline. The agent writes a non-production version first.

Use **action handoff** when another person or team owns the next step. The agent creates a task with the required context.

{% hint style="warning" %}
Do not use agent instructions as the only protection for high-risk actions. Instructions guide model behavior, but they do not enforce authorization or business rules.
{% endhint %}

### Soft controls for intent clarification

A soft control instructs the agent to request confirmation or missing details. It is useful when the request is ambiguous, but the eventual action is low risk.

Define the behavior in the agent instructions or in a tool saga's AI description. The tool description should name the ambiguity and state the expected question.

{% code overflow="wrap" %}
```
Add To Basket Saga → Saga Definition → AI tab → Tool Description

Adds a product to the customer's basket.
If the customer does not specify basket or favorites, ask which destination to use.
Do not call this tool until the customer confirms the destination.
```
{% endcode %}

This pattern improves the conversation, but it is not an enforcement boundary. The model can still misunderstand or fail to follow an instruction.

### Hard approvals for explicit authorization

Hard approvals pause the agent loop before a selected tool call executes. Rierino returns the proposed action as a candidate for the end user to approve or reject.

Configure an approval pattern on the tool saga in **Saga Definition → AI**. The pattern can be conditional, so only actions that meet a risk threshold require approval.

{% code overflow="wrap" %}
```
Submit Order Saga → Saga Definition → AI tab → Approval Pattern

order.value > 100
```
{% endcode %}

The user reviews the proposed action during the conversation. The agent continues only after approval. A rejection prevents that action from executing.

Hard approvals work best when the person who chats with the agent can approve the action. They suit real-time workflows, such as submitting a costly order or changing customer account details.

Key decision data can be included in the approval message using saga explainer template. For example, show the order total, supplier, and delivery date before requesting approval.

### Draft actions for offline review

Draft actions separate generation from publication. The agent creates an interim business record, then an authorized user reviews and promotes it.

Configure a draft state manager for agent output. Give the agent access to the draft state only. Use a separate production state manager and user-facing action for the publish transition.

{% code overflow="wrap" %}
```
Create Blog Saga
  → Write article to Draft_Blog state
  → Editor reviews the draft
  → Editor publishes it to Blog state
```
{% endcode %}

This pattern provides a clear review boundary and preserves the generated output for inspection. It is especially useful when content requires detailed review or approval happens after the chat ends.

### Action handoff for team-owned work

Action handoff assigns the next step to a user or team. The agent does not perform the final action. Instead, it calls a tool saga that assigns a task with the context gathered during the interaction.

The assignee completes the work outside the chat. Their outcome can trigger a later saga or agent interaction to continue the workflow.

{% code overflow="wrap" %}
```
Request Order Saga
  → Create task for Procurement
  → Procurement creates the order
  → Return the order number
  → Resume the agent workflow
```
{% endcode %}

Use action handoff when the requester and decision-maker are different people. It also works when the action needs specialist review, separation of duties, or asynchronous processing.

Include enough task context for the assignee to act without reopening the chat. Capture the requested action, relevant record identifiers, generated rationale, and deadline.

### Design a reliable approval workflow

1. **Classify each tool action.** Identify its business impact, reversibility, and required authority.
2. **Enforce non-negotiable rules in the saga.** Use hard controls for authorization and business constraints.
3. **Require approval at meaningful thresholds.** Use conditional hard approvals for high-value or sensitive actions.
4. **Keep an offline path for long reviews.** Use drafts or handoffs when a live response is not practical.

Review the tools available to each agent in [.](./ "mention"). Grant only the capabilities the agent needs. Then apply the appropriate HITL pattern to every consequential action.
