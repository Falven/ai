---
'@ai-sdk/policy': patch
---

Introduce `@ai-sdk/policy`, an Open Policy Agent adapter for the `toolApproval`
callback on `generateText` / `streamText` / `ToolLoopAgent`.

Engine-neutral root exports a `PolicyClient` interface, `shadow()` for safe
policy rollout with fire-and-forget telemetry, and `wrapMcpTools()` for making
approval configuration total over a discovered tool surface. The `./opa`
subpath ships `opaPolicy` / `optionalOpaPolicy` (Rego-as-code authorization),
`wasmPolicyClient` and `httpPolicyClient` backends (lazy-loaded optional peer
deps), `opaCapabilityMiddleware` for fail-closed model-level tool filtering,
and `normalizeOpaDecision` for users who call OPA themselves.

Sits entirely on top of the public SDK surface, with no changes to `ai`,
`@ai-sdk/provider`, or `@ai-sdk/provider-utils`. Transitive enforcement
(coarse dispatchers like `bash` / `http.request` / MCP proxies) is handled
inside the user's `toolApproval` by parsing the dispatcher input and routing
to the same Rego rule that gates the direct tool.
