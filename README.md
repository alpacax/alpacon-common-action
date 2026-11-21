# Alpacon Common Action

[![GitHub marketplace](https://img.shields.io/badge/marketplace-alpacon--common--action-blue?logo=github)](https://github.com/marketplace/actions/alpacon-common-action)

Run any [Alpacon](https://alpacon.io) CLI command in your GitHub Actions workflow—a flexible escape hatch for operations not covered by the specialized actions.

- [Official documentation](https://docs.alpacax.com/reference/actions/common/)

## Why use this action?

- **Full CLI access**—Run any `alpacon` command, not just websh or cp
- **Automation friendly**—List servers, check status, manage groups, and view command history from CI/CD
- **Consistent auth**—Uses the same service token workflow as other Alpacon actions
- **Composable**—Combine with other Alpacon actions for complex automation pipelines

## How access is controlled

This action runs on the service-token path, which is separate from the work sessions that people and AI agents open:

- **Reviewed before it works**—A service token that can run commands or transfer files is approved in the Alpacon console before it authenticates anywhere.
- **Deny by default**—Its ACL names the servers, commands and file paths the pipeline may use. Everything else is refused.
- **No one waits**—Within that ACL the workflow runs without a work session or an approval step.
- **Bounded**—Optional allowed IP ranges and hours, and an expiry set by the token's rotation policy.
- **On the record**—Every command and file transfer is recorded under the token in the workspace audit log.

## Authentication

Set `service-token` to an Alpacon service token (its key starts with `alpst-`). A service token belongs to an IAM Application (a machine identity), not to a person, so it keeps working when people leave. Its access is limited by the token's scopes and its server, command, and file access rules. A service token does not need a Work Session; endpoint permissions still apply.

Store the token as a repository secret named `ALPACON_SERVICE_TOKEN`, as in the examples below. Logging in with a service token requires Alpacon CLI v1.7.4 or later.

See [Service tokens](https://docs.alpacax.com/use/iam/applications/service-tokens/) and the [GitHub Actions integration guide](https://docs.alpacax.com/integrate/github-actions/).

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
        uses: alpacax/alpacon-common-action@v2
        with:
          workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
          service-token: ${{ secrets.ALPACON_SERVICE_TOKEN }}
          command: "server ls"
```

### List groups

```yaml
- name: List groups
  uses: alpacax/alpacon-common-action@v2
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    service-token: ${{ secrets.ALPACON_SERVICE_TOKEN }}
    command: "group ls"
```

### View recent command executions

```yaml
- name: View recent command executions
  uses: alpacax/alpacon-common-action@v2
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    service-token: ${{ secrets.ALPACON_SERVICE_TOKEN }}
    command: "exec ls --tail=5"
```

## Inputs

| Name | Description | Required |
|------|-------------|----------|
| `workspace-url` | Alpacon workspace URL | Yes |
| `service-token` | Alpacon service token key (`alpst-`) | Yes |
| `command` | Alpacon command to execute (without `alpacon` prefix) | Yes |

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| `alpacon: command not found` | CLI not installed | Add `alpacax/alpacon-setup-action@v1` before this action |
| `login failed` | Invalid credentials | Verify `workspace-url` and `service-token` secrets are set correctly |
| `unknown command` | Typo or unsupported command | Check the [CLI command list](https://docs.alpacax.com/reference/cli/) |

## Related actions

- [alpacon-setup-action](https://github.com/alpacax/alpacon-setup-action)—Install Alpacon CLI (required)
- [alpacon-exec-action](https://github.com/alpacax/alpacon-exec-action)—Execute shell commands on remote servers
- [alpacon-cp-action](https://github.com/alpacax/alpacon-cp-action)—Copy files to/from remote servers

## Resources

- [GitHub Actions integration guide](https://docs.alpacax.com/integrate/github-actions/)
- [Alpacon CLI reference](https://docs.alpacax.com/reference/cli/)

## Releasing

When creating a new release, always update the `v2` major version tag:

```bash
git tag -f v2 v2.x.0
git push origin v2 --force
```

This ensures users referencing `@v2` automatically get the latest release.
