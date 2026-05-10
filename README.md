# mcp-guard demo

This repository demonstrates `mcp-guard` as a GitHub Action on a real pull request.

The main branch contains a safe `.mcp.json` config. The demo pull request changes that config to risky MCP server entries so `mcp-guard` can block the change and generate Markdown, HTML, JSON, and SARIF reports.

## What to Inspect

- Workflow: `.github/workflows/mcp-guard.yml`
- Safe config on `main`: `.mcp.json`
- Intentional failing demo PR: https://github.com/ChaoYue0307/mcp-guard-demo/pull/1
- Marketplace Action: https://github.com/marketplace/actions/mcp-guard-mcp-security-scanner
- Product site and transparent example: https://chaoyue0307.github.io/mcp-guard/e2e/

## Workflow

```yaml
- uses: ChaoYue0307/mcp-guard-action@v0.4.2
  with:
    config: .mcp.json
    fail-on: high
    comment-pr: "true"
    upload-sarif: "true"
```

## Expected Demo Behavior

On `main`, the workflow should pass because the config uses a local checked-in command and a narrow working directory.

On the unsafe demo pull request, the workflow should fail because the PR introduces:

- `bash -c` with curl-pipe-shell startup
- unpinned `npx` package execution
- root filesystem access
- secret-like environment variables and headers
- remote MCP endpoint configuration

That failed check is intentional. It shows how a team can catch risky agent tool config before merge.
