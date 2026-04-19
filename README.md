# AgentGateway (agentgateway)
AgentGateway is an open-source, AI-native proxy and gateway for routing, observing, and governing traffic to and from AI agents, LLM providers, and MCP servers. Built on the A2A and MCP protocols, it provides a unified gateway for LLM consumption, MCP tool federation, agent-to-agent communication, security, and observability. AgentGateway supports multi-provider LLM routing across OpenAI, Anthropic, Google Gemini, AWS Bedrock, and Azure OpenAI with built-in RBAC, JWT authentication, rate limiting, and OpenTelemetry integration.

**URL:** [https://agentgateway.dev/](https://agentgateway.dev/)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AI Gateway, API Gateway, MCP, LLM, Agent-to-Agent, Open Source, CNCF, Observability, Security

## Timestamps

- **Created:** 2026-03-27
- **Modified:** 2026-04-19

## APIs

### AgentGateway
AgentGateway provides AI-native gateway capabilities for routing LLM traffic, federating MCP tools, enabling agent-to-agent communication, and applying security and observability controls across AI agent infrastructure.

**Human URL:** [https://agentgateway.dev/](https://agentgateway.dev/)

#### Tags:

 - AI Gateway, LLM Routing, MCP, Agent-to-Agent, Security, Observability

#### Properties

- [Documentation](https://agentgateway.dev/docs/)
- [GettingStarted](https://agentgateway.dev/docs/quickstart/)
- [GitHubRepository](https://github.com/agentgateway/agentgateway)

## Common Properties

- [GitHubOrganization](https://github.com/agentgateway)
- [JSONSchema - LLM Backend Schema](https://raw.githubusercontent.com/api-evangelist/agentgateway/refs/heads/main/json-schema/agentgateway-llm-backend-schema.json)
- [JSONSchema - MCP Target Schema](https://raw.githubusercontent.com/api-evangelist/agentgateway/refs/heads/main/json-schema/agentgateway-mcp-target-schema.json)
- [JSONSchema - Route Schema](https://raw.githubusercontent.com/api-evangelist/agentgateway/refs/heads/main/json-schema/agentgateway-route-schema.json)
- [JSON-LD](https://raw.githubusercontent.com/api-evangelist/agentgateway/refs/heads/main/json-ld/agentgateway-context.jsonld)
- [Vocabulary](https://raw.githubusercontent.com/api-evangelist/agentgateway/refs/heads/main/vocabulary/agentgateway-vocabulary.yaml)
- [Portal](https://agentgateway.dev/)
- [Documentation](https://agentgateway.dev/docs/)
- [GettingStarted](https://agentgateway.dev/docs/quickstart/)
- [Support](https://discord.gg/y9efgEmppm)

## Features

| Name | Description |
|------|-------------|
| LLM Gateway | Routes traffic to OpenAI, Anthropic, Google Gemini, AWS Bedrock, and Azure OpenAI through a unified API with model aliasing, failover, and load balancing. |
| MCP Gateway | Connects LLMs to tools via Model Context Protocol with static and dynamic routing, tool federation, and stateful MCP sessions. |
| Agent-to-Agent (A2A) Gateway | Enables secure, governed communication between AI agents using the A2A protocol for multi-agent orchestration. |
| Inference Routing | Intelligently routes requests to self-hosted models based on GPU utilization and request priority. |
| Security and Authentication | Provides JWT, OAuth2, API key management, CORS, CSRF protection, MCP authentication, and external authorization support. |
| Traffic Management | Supports request routing and matching, header manipulation, rate limiting, retries, gRPC routing, traffic splitting, and direct responses. |
| Observability | Integrates with OpenTelemetry for metrics, traces, and access logging with a built-in Admin UI and debugging tools. |
| Guardrails | Applies prompt guards, content filtering, regex filters, moderation policies, and custom webhooks for AI safety. |
| Cost Controls | Tracks budget and spend limits per user, team, or application with RBAC-based controls on LLM consumption. |
| Prompt Enrichment | Supports prompt templates and enrichment for standardizing and augmenting requests before routing to LLM providers. |

## Use Cases

| Name | Description |
|------|-------------|
| Unified LLM Routing | Route requests across multiple LLM providers with a single API, enabling failover, load balancing, and cost optimization without changing client code. |
| MCP Tool Federation | Aggregate tools from multiple MCP servers behind a single gateway endpoint, enabling agents to discover and invoke tools from any connected MCP server. |
| Enterprise AI Governance | Apply organization-wide security policies, rate limits, budget controls, and content filters to all AI agent traffic through a centralized gateway. |
| REST API to MCP Conversion | Convert existing REST APIs into MCP-native tool endpoints that AI agents can discover and invoke through the Model Context Protocol. |
| Multi-Agent Orchestration | Enable secure agent-to-agent communication using the A2A protocol, allowing specialized agents to delegate tasks to each other through the gateway. |
| Observability and Debugging | Collect unified telemetry across all AI agent and LLM interactions to monitor cost, latency, and behavior at scale. |

## Integrations

| Name | Description |
|------|-------------|
| OpenAI | Route to OpenAI GPT models through the AgentGateway LLM backend with model aliasing and budget controls. |
| Anthropic | Connect to Anthropic Claude models via the unified LLM gateway with failover and load balancing. |
| Google Gemini | Route traffic to Google Gemini models through the AgentGateway multi-provider backend. |
| AWS Bedrock | Integrate with AWS Bedrock for managed LLM access via the AgentGateway routing layer. |
| Azure OpenAI | Route requests to Azure-hosted OpenAI models through the unified gateway API. |
| Ollama | Connect to locally hosted Ollama models for self-hosted inference routing. |
| vLLM | Route to vLLM inference servers with GPU utilization-aware routing for optimal performance. |
| OpenTelemetry | Export metrics, traces, and logs to any OpenTelemetry-compatible observability backend. |
| Kubernetes Gateway API | Deploy and configure AgentGateway on Kubernetes using the standard Gateway API for dynamic configuration. |

## Artifacts

Machine-readable API specifications organized by format.

### JSON Schema

- [LLM Backend Schema](json-schema/agentgateway-llm-backend-schema.json)
- [MCP Target Schema](json-schema/agentgateway-mcp-target-schema.json)
- [Route Schema](json-schema/agentgateway-route-schema.json)

### JSON Structure

- [LLM Backend Structure](json-structure/agentgateway-llm-backend-structure.json)
- [MCP Target Structure](json-structure/agentgateway-mcp-target-structure.json)
- [Route Structure](json-structure/agentgateway-route-structure.json)

### JSON-LD

- [AgentGateway Context](json-ld/agentgateway-context.jsonld)

## Vocabulary

- [AgentGateway Vocabulary](vocabulary/agentgateway-vocabulary.yaml) — Unified taxonomy mapping 6 resources, 7 actions, 3 workflows, and 3 personas across operational and capability dimensions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
