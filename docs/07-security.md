# Security, Trust, and Permissions

## 1. Security objectives

Protect source code, credentials, local files, execution environments, agent sessions, graph knowledge, and external accounts. Treat repository content, model output, extension packages, and tool responses as untrusted input unless explicitly validated.

## 2. Threat boundaries

Threat modeling must cover:
- malicious or compromised repositories;
- prompt injection in code, docs, issues, web pages, and tool output;
- malicious extensions and MCP servers;
- compromised model/provider credentials;
- cross-project data leakage;
- unauthorized local IPC access;
- command execution and filesystem escape;
- poisoned or stale knowledge;
- agent-to-agent privilege escalation;
- supply-chain compromise and update channels.

## 3. Permission model

Use explicit capability-based permissions. Separate read, write, execute, network, secret access, repository mutation, and external side-effect permissions.

Permissions should be scoped by project, workspace, task, tool, and duration where practical. Default to least privilege. High-impact operations require explicit approval according to user policy.

## 4. Repository trust

Repository-provided setup scripts, hooks, workflows, agent commands, and configuration are executable code. Inspect and show requested actions before trusting them. Trust should be scoped and invalidated when trusted content changes.

Shipyard's documented project configuration model is a useful reference: it distinguishes repository-supplied executable configuration from user trust and re-trusts changed content. Evaluate compatibility rather than copying assumptions blindly.

## 5. Sidecar and IPC

- Bind only to local IPC or authenticated loopback.
- Use OS-level access control where available.
- Authenticate local clients; do not rely on obscurity of socket paths or ports.
- Validate protocol versions, payload sizes, paths, and identifiers.
- Restrict database paths to approved application directories.
- Avoid arbitrary path traversal and arbitrary command execution through intelligence APIs.
- Redact credentials and sensitive prompt content from logs.

## 6. Data separation

Enforce project boundaries in all queries and writes. A retrieval request for one project must not leak another project's graph or evidence. Define explicit export, deletion, retention, and backup behavior.

## 7. Extension sandboxing

Extensions should run with declared permissions and resource budgets. Prefer process isolation for untrusted or high-risk extensions. Validate signatures/integrity and support revocation. Never treat an extension manifest as proof that the package is safe.

## 8. Knowledge integrity

Store provenance and source revisions. Detect stale records. Distinguish user-approved decisions from inferred facts and model-generated suggestions. Prevent an agent from silently promoting its own output to accepted project policy.

## 9. Security verification

Security tests should include:
- malicious repository configuration;
- prompt-injection content in retrieved files;
- cross-project query isolation;
- unauthorized IPC requests;
- path traversal and symlink edge cases;
- permission escalation attempts;
- secret leakage in logs and context packets;
- extension signature/version failures;
- cancellation and cleanup after interrupted commands.
