# Unified Extensions Ecosystem

## 1. Direction

Use one coherent extension ecosystem rather than unrelated plugin mechanisms. Core capabilities remain native to the platform; extensions add optional integrations and specialized functionality.

## 2. Extension categories

- **Native Tools:** built-in deterministic capabilities exposed through stable tool contracts.
- **MCP Servers:** external tool servers or MCP exposure of approved internal capabilities.
- **Skills:** reusable instructions, procedures, and domain knowledge.
- **Agent Packages:** specialist role definitions, capabilities, contracts, and required tools.
- **Workflow Packages:** reusable task flows, gates, and orchestration templates.
- **Intelligence Providers:** indexing, retrieval, analysis, or ranking integrations.
- **Memory Providers:** optional storage/retrieval integrations, without replacing the approved core graph decision.
- **Model Providers:** model APIs, local model runtimes, and inference services.
- **UI Plugins:** user-interface extensions with explicit permissions.

## 3. Manifest and compatibility

Every installable package should declare:
- unique ID, version, publisher, description, and license;
- extension type and entry point;
- supported platform and runtime versions;
- capabilities and required permissions;
- tools, models, and external services required;
- dependencies and version constraints;
- configuration schema and secret requirements;
- resource expectations;
- integrity/signature metadata where supported.

Dependency resolution must detect conflicts, unsupported versions, missing requirements, and cycles.

## 4. Permissions and lifecycle

The platform should support install, enable, disable, update, rollback, uninstall, and audit. Permissions should be least-privilege and visible before installation or execution. Untrusted extensions must not receive unrestricted filesystem, process, network, or secret access.

## 5. MCP's role

MCP is an interoperability protocol, not the architecture of the whole product. Use MCP when it improves interoperability with external tools or allows other clients to access approved capabilities. Do not require users to install MCP servers for core native features.

Internal domain services should have stable APIs independent of MCP. MCP adapters can expose selected operations without leaking internal implementation details.

## 6. Marketplace

A future catalog/marketplace may support discovery, publisher identity, compatibility checks, ratings, version history, signatures, and revocation. Do not assume marketplace availability in the initial release.

## 7. Governance

Define package trust levels, review requirements, security scanning, provenance, vulnerability reporting, and emergency revocation before enabling third-party execution at scale.
