# Alpacon Common Action

Run any Alpacon CLI command in your Alpacon workspace using this GitHub Action.

- Official Docs: [Alpacon CLI Guide](https://docs.alpacax.com/alpacon/cli)

## Features

- Execute any `alpacon` command with full flexibility
- Handles authentication and workspace connection automatically

## Prerequisites

This action requires the Alpacon CLI to be installed in your workflow. Use the [Alpacon Setup Action](https://github.com/marketplace/actions/alpacon-setup-action) first:

```yaml
- name: Setup Alpacon CLI
  uses: alpacax/alpacon-setup-action@v1
```

## Usage examples

### List servers

```yaml
- name: List Servers
  uses: alpacax/alpacon-common-action@v1
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    api-token: ${{ secrets.ALPACON_API_TOKEN }}
    command: "server ls"
```

### List groups

```yaml
- name: List Groups
  uses: alpacax/alpacon-common-action@v1
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    api-token: ${{ secrets.ALPACON_API_TOKEN }}
    command: "group ls"
```

### View recent events

```yaml
- name: View Recent Events
  uses: alpacax/alpacon-common-action@v1
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    api-token: ${{ secrets.ALPACON_API_TOKEN }}
    command: "event --tail=5"
```

### Run a remote command via websh

```yaml
- name: Run Remote Command
  uses: alpacax/alpacon-common-action@v1
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    api-token: ${{ secrets.ALPACON_API_TOKEN }}
    command: 'websh my-server echo "Hello from CI"'
```

## Inputs

| Name           | Description                                 | Required |
|----------------|---------------------------------------------|----------|
| workspace-url  | Alpacon workspace URL.                      | Yes      |
| api-token      | Alpacon API token for authentication.       | Yes      |
| command        | Alpacon command to execute.                 | Yes      |

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| `alpacon: command not found` | CLI not installed | Add `alpacax/alpacon-setup-action@v1` before this action |
| `login failed` | Invalid credentials | Verify `workspace-url` and `api-token` secrets are set correctly |
| `unknown command` | Typo or unsupported command | Check the [CLI command list](https://docs.alpacax.com/alpacon/cli) |

## Notes

- See [Alpacon CLI command list](https://docs.alpacax.com/alpacon/cli)
