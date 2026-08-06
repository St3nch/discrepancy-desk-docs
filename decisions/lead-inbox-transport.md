# Lead Inbox Transport

D18. Decided before ticket 09 opened, because the handoff from ticket 08 identified a
tension between D15's tool surface and D10's product description that would otherwise
have been resolved silently by whoever coded first.

---

## The tension

**D15 lists `add_lead` on the eight-call executor tool surface.** That surface is the
complete interface between an executor and the backend.

**D10 describes the lead inbox as an operator workflow.** A URL encountered while
browsing, a podcast heard by chance, a link from a conversation — ambient discovery,
needing somewhere to land that is not "open a whole case."

Those pull in opposite directions, and by ticket 08 the codebase had established a
convention that makes the question sharper: **`MCP_AND_API` is empty.** Every operation
is deliberately on one surface or the other. Nothing sits in both.

---

## D18 — `add_lead` is the first `MCP_AND_API` entry

The operator drops leads from the browser. An executor may also drop one, mid-run, when
it encounters material outside its run's question.

**Why both rather than one.**

The operator case is the primary one and needs no argument — D10 exists to serve it.

The executor case is real and currently has no good answer. A run scoped to "what did
the official investigation conclude" that encounters a promising unrelated source has
exactly two options today: capture it against the wrong run, polluting that run's
material and consuming its budget, or discard it. Both are worse than parking it.

**Why this does not weaken the transport rule.**

The rule exists because human-authority operations must never be reachable from MCP.
The list is specific: setting authoritative evidence dimensions, resolving entity
identity, classifying publication risk, ruling a connection publishable, approving
content, dispatching a run.

**Dropping a URL commits nothing.** A lead holds material and never claims (D10). It
belongs to no case. It asserts nothing about the world. Nothing downstream can cite it
until a human attaches it to a case and a run works it. The reason MCP is kept narrow
is that its refusals are the product's boundary — and there is no boundary here to
cross.

**What stays API-only.** Everything that follows the drop: attaching a lead to a case,
promoting it to a new case, disposing of it. Those are editorial judgements about what
material is worth pursuing, and they are the human's.

| Operation | Surface |
|---|---|
| `add_lead` | **MCP_AND_API** |
| attach lead to case | API only |
| promote lead to new case | API only |
| dispose of lead | API only |
| summarise lead (optional) | API only |

---

## Rejected — API only

Simpler, and preserves the empty-`MCP_AND_API` convention unbroken. Rejected because it
leaves the executor with no way to park out-of-scope material, and the workarounds are
both harmful: capture against the wrong run, or lose the find.

## Rejected — MCP only

Matches D15 literally and is obviously wrong in practice. The operator is the primary
lead source, and requiring a running executor session to drop a URL found while
browsing defeats the purpose of an inbox.

---

## Consequence

**The empty `MCP_AND_API` convention is broken deliberately, once, with reasons
attached.** It was a strong signal precisely because it was empty, and this decision
spends some of that. The convention was never a rule in itself — the rule is that a
service function is wired to one surface or the other and never assumed safe on both.
`add_lead` is assessed and found safe on both, which is what the rule asks for.

A second entry should face the same scrutiny and should not cite this one as
precedent. "D18 put something in `MCP_AND_API`" is not an argument.

---

## Implementation notes for ticket 09

**Lead capture and run capture must produce identical capture records** for the same
URL — same store, same hash, same parse, same element structure. Reuse `safe_http_get`
and `assert_content_type_supported` rather than forking the fetch path. A lead captured
today and the same URL captured under a run tomorrow are two captures of one artifact,
and nothing downstream should be able to tell which door they came through.

**`captures.run_id` is NOT NULL today.** A lead has no run. This needs a nullable
`run_id` or a `lead_id`, and a migration. Design it before writing fetch code.

**A lead's capture status is `unexamined`.** There is no run to close and therefore
nothing to mark it examined. That is honest — nobody has looked — and it is where that
vocabulary earns its keep after tickets 05 and 08 established that `examined` is
reported rather than inferred (F-32).

**Auth-walled and paywalled URLs are identity-only leads, explicitly marked as not
captured.** This is a product state and is distinct from an SSRF refusal, which is
fail-closed enforcement. Storing a login wall as though it were an artifact is worse
than storing nothing, because it masquerades as evidence.
