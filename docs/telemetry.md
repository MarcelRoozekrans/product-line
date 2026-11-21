# Telemetry conventions

Every product in the line records agent activity with the same OpenTelemetry names, so one product (Themis.NET first) can read what another records. Products stay vendor-neutral: they emit OpenTelemetry and never choose a backend.

Each product copies the names it needs. There is no shared package.

## Version

These conventions follow the OpenTelemetry GenAI semantic conventions as published in **semantic conventions v1.41.0**, the last core release that contained them. In v1.42.0 (June 2026) OpenTelemetry moved the GenAI conventions to the repository `open-telemetry/semantic-conventions-genai`; they are still in Development status there.

Moving to a newer version is a change to this document. Each product follows in its own release, and an attribute that is renamed keeps its old name for an overlap period (see Renames).

## Which signal for what

| What happens | Signal | Name |
|---|---|---|
| An agent run | span | `invoke_agent {gen_ai.agent.name}` |
| A tool call | span | `execute_tool {gen_ai.tool.name}` |
| A model call | span | `{gen_ai.operation.name} {gen_ai.request.model}`, for example `chat gpt-4` or `embeddings text-embedding-3-small` |
| A retrieval from a vector store | span | `retrieval {gen_ai.data_source.id}` |
| Duration of a model call | metric (histogram, seconds) | `gen_ai.client.operation.duration` |
| Tokens used | metric (histogram) | `gen_ai.client.token.usage` |
| An audit entry (a scan, a block, an approval) | log record | product-prefixed attributes, see below |

Every span sets `gen_ai.operation.name` to the operation it represents, and the span name starts with that same value: `invoke_agent`, `execute_tool` and `retrieval` as above, and for a model call the operation of the request, such as `chat` or `embeddings`. A span for a tool call sets `gen_ai.tool.name`, and `gen_ai.tool.call.id` when the CLI or model gives one. A span for an agent run sets `gen_ai.agent.name`.

Durations are in seconds.

## Product attributes

An attribute that a product defines itself carries the product's prefix. Attributes defined by an OpenTelemetry convention, the GenAI ones or any other such as `http.request.method`, keep their standard names.

| Product | Prefix |
|---|---|
| AI.Sentinel | `ai.sentinel.` |
| Rag.NET | `ragnet.` |
| Themis.NET | `themis.` |
| Thalos.NET | `thalos.` |
| Mnemosyne.NET | `mnemosyne.` |
| Minos.NET | `minos.` |
| Ariadne.NET | `ariadne.` |
| MagicLink.NET | `magiclink.` |

A new product adds its prefix here before it emits its own attributes.

## Log records belong to a trace

A log record about something that happened inside a traced operation carries that operation's trace id and span id. For AI.Sentinel this means every audit entry about a tool call carries the trace and span id of that tool call, so it shows up with the call in one trace, also when the hook runs in a separate process from the CLI. A hook that runs in its own process has no active span of its own. The CLI plugin passes the context of the tool-call span to the hook as a W3C `traceparent`, so the parent id in it is the tool call's span id, not the agent run's. When no `traceparent` reaches the hook, the audit entry is still recorded, without a trace id or span id.

## Content

Prompt text, model responses and tool inputs and outputs are not recorded by default. A product may record them only behind an explicit opt-in, and then under the GenAI content attributes.

## Renames

When an attribute is renamed, the product emits both the old and the new name for at least one minor release, and its release notes say when the old name goes. Removing the old name is a breaking change for anyone reading it.
