
# Alpacon Common Action

Run any Alpacon CLI command in your Alpacon workspace using this GitHub Action.

- Official Docs: [Alpacon CLI Guide](https://docs.alpacax.com/alpacon/cli)

## Features

- Execute any `alpacon` command with full flexibility
- Handles authentication and workspace connection automatically

## Usage examples

```yaml
- name: Run Alpacon Command
  uses: alpacax/alpacon-common-action@v1
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
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
