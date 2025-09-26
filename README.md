
# Alpacon Common Action

Run any Alpacon CLI command in your AlpacaX workspace using this GitHub Action.

- Official Docs: [Alpacon CLI Guide](https://docs.alpacax.com/alpacon/cli)

## Features

- Execute any `alpacon` command with full flexibility
- Handles authentication and workspace connection automatically

## Prerequisites

This action requires the Alpacon CLI to be installed in your workflow. Use the [Alpacon Setup Action](https://github.com/marketplace/actions/alpacon-setup-action) first:

```yaml
- name: Setup Alpacon CLI
  uses: alpacax/alpacon-setup-action@v1.0.0
```

## Usage Example

```yaml
- name: Run Alpacon Command
  uses: alpacax/alpacon-common-action@v1.0.0
  with:
    workspace-url: ${{ secrets.ALPACAX_WORKSPACE_URL }}
    api-token: ${{ secrets.ALPACON_API_TOKEN }}
    command: "server ls"
```

## Inputs

| Name           | Description                                 | Required |
|----------------|---------------------------------------------|----------|
| workspace-url  | Alpacon workspace URL.                      | Yes      |
| api-token      | Alpacon API token for authentication.       | Yes      |
| command        | Alpacon command to execute.                 | Yes      |

## Notes

- See [Alpacon CLI command list](https://docs.alpacax.com/alpacon/cli)
- Example: `command: "group ls"`, `command: "note create --title 'test' --body 'hello'"` and more
