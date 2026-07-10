# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 0.27.x (latest) | :white_check_mark: |
| < 0.27.x | :x:              |

## Reporting a Vulnerability

We take security vulnerabilities seriously. If you discover a security issue, please report it responsibly.

### How to Report

**Please DO NOT open a public GitHub issue for security vulnerabilities.**

Instead, please email us at: **security@headroomlabs.ai**

Include the following information:
- Type of vulnerability (e.g., injection, data exposure, authentication bypass)
- Full path of the affected source file(s)
- Step-by-step instructions to reproduce the issue
- Proof-of-concept or exploit code (if possible)
- Impact assessment

### What to Expect

1. **Acknowledgment**: We will acknowledge receipt within 48 hours
2. **Assessment**: We will assess the vulnerability and determine its severity
3. **Updates**: We will keep you informed of our progress
4. **Resolution**: We aim to resolve critical issues within 7 days
5. **Credit**: With your permission, we will credit you in the security advisory

### Security Best Practices for Users

When using Headroom:

1. **API Keys**: Never commit API keys. Use environment variables.
2. **Proxy Exposure**: Don't expose the proxy server to the public internet without authentication
3. **Log Files**: Be aware that request logs may contain sensitive information
4. **Budget Limits**: Set budget limits to prevent unexpected costs

### Scope

The following are in scope for security reports:
- Headroom Python package (`pip install headroom-ai`)
- Headroom proxy server
- Official integrations (LangChain, Agno, Strands, LiteLLM, Vercel AI SDK, Anthropic/OpenAI SDK wrappers, MCP)

The following are out of scope:
- Third-party integrations not maintained by us
- Issues in dependencies (report these to the upstream project)
- Social engineering attacks

## Security Features

Headroom includes several security features:

- **No credential storage**: We never store or log API keys
- **Passthrough mode**: Sensitive content passes through unchanged by default
- **Input validation**: All inputs are validated before processing
- **Safe defaults**: Security-conscious defaults out of the box

## Security Audit History

### 2026-07-10 — Internal code review (commit `5e14b8c0f293df78f2576a9fc7eb90189e604cb5`)

- **Scope**: Full-repository review for reverse shells, malicious code/logic, and data-exfiltration indicators (not a diff/PR review).
- **Result**: No malicious code found. Command execution throughout the hook/wrap install path uses list-argv `subprocess` calls (no `shell=True`, no `eval`/`exec`/`os.system` on untrusted input); proxy egress is limited to the configured upstream LLM provider or user-specified endpoints; telemetry is opt-out-able and sends only aggregate/hashed stats, never prompt content or credentials; no hidden persistence, credential harvesting, or obfuscated payloads were found.
- **Hardening notes (not vulnerabilities)**:
  - The companion `rtk` binary (fetched from GitHub releases via `headroom/rtk/installer.py`) and the Docker image pulled by `scripts/install.sh` are not checksum/signature-verified before execution — integrity currently relies on HTTPS/registry trust.
  - Optional "Cloud mode" (enabled by setting `HEADROOM_API_KEY`) sends full message content to `api.headroomlabs.ai`; this is documented and disabled by default.
- Reviewed by: Claude Code, automated audit commissioned by mdaue@rapta.ai.

Thank you for helping keep Headroom and its users safe!
