# Alpacon Common Action

[![GitHub marketplace](https://img.shields.io/badge/marketplace-alpacon--common--action-blue?logo=github)](https://github.com/marketplace/actions/alpacon-common-action)

Run any [Alpacon](https://alpacon.io) CLI command in your GitHub Actions workflow — a flexible escape hatch for operations not covered by the specialized actions.

- Official Docs: [alpacon-common-action reference](https://docs.alpacax.com/reference/actions/common/)

## Why use this action?

- **Full CLI access** — Run any `alpacon` command, not just websh or cp
- **Automation friendly** — List servers, check status, manage groups, and view events from CI/CD
- **Consistent auth** — Uses the same API token workflow as other Alpacon actions
- **Composable** — Combine with other Alpacon actions for complex automation pipelines

## Usage

### List servers

```yaml
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - name: Setup Alpacon CLI
        uses: alpacax/alpacon-setup-action@v1

      - name: List servers
        uses: alpacax/alpacon-common-action@v1
        with:
          workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
          api-token: ${{ secrets.ALPACON_API_TOKEN }}
          command: "server ls"
```

### List groups

```yaml
- name: List groups
  uses: alpacax/alpacon-common-action@v1
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    api-token: ${{ secrets.ALPACON_API_TOKEN }}
    command: "group ls"
```

### View recent events

```yaml
- name: View recent events
  uses: alpacax/alpacon-common-action@v1
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    api-token: ${{ secrets.ALPACON_API_TOKEN }}
    command: "event --tail=5"
```

## Inputs

| Name | Description | Required |
|------|-------------|----------|
| `workspace-url` | Alpacon workspace URL | Yes |
| `api-token` | Alpacon API token for authentication | Yes |
| `command` | Alpacon command to execute (without `alpacon` prefix) | Yes |

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| `alpacon: command not found` | CLI not installed | Add `alpacax/alpacon-setup-action@v1` before this action |
| `login failed` | Invalid credentials | Verify `workspace-url` and `api-token` secrets are set correctly |
| `unknown command` | Typo or unsupported command | Check the [CLI command list](https://docs.alpacax.com/reference/cli/) |

## Related actions

- [alpacon-setup-action](https://github.com/alpacax/alpacon-setup-action) — Install Alpacon CLI (required)
- [alpacon-websh-action](https://github.com/alpacax/alpacon-websh-action) — Execute shell commands on remote servers
- [alpacon-cp-action](https://github.com/alpacax/alpacon-cp-action) — Copy files to/from remote servers

## Resources

- [GitHub Actions integration guide](https://docs.alpacax.com/integrate/github-actions/)
- [Alpacon CLI reference](https://docs.alpacax.com/reference/cli/)

## Releasing

When creating a new release, always update the `v1` major version tag:

```bash
git tag -f v1 v1.x.0
git push origin v1 --force
```

This ensures users referencing `@v1` automatically get the latest release.
