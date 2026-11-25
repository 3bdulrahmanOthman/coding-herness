# ADR-006: Unified Extensions Ecosystem

- Status: **Accepted**
- Date: 2026-11-25

## Context

The platform needs extensibility across tools, agents, workflows, models, intelligence providers, and UI without fragmenting into incompatible plugin systems.

## Decision

Adopt a Unified Extensions Ecosystem with typed manifests, capability declarations, dependencies, compatibility metadata, permissions, and lifecycle management.

Categories include Native Tools, MCP Servers, Skills, Agent Packages, Workflow Packages, Intelligence Providers, Memory Providers, Model Providers, and UI Plugins.

## Consequences

- Core capabilities remain native; MCP is an interoperability mechanism, not a prerequisite for core functionality.
- Extension install/update/disable/rollback/uninstall must be governed.
- Permissions and package trust must be explicit.
- A marketplace is a future capability, not an initial requirement.
