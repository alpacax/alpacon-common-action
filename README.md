# Alpacon Common Action

[![GitHub marketplace](https://img.shields.io/badge/marketplace-alpacon--common--action-blue?logo=github)](https://github.com/marketplace/actions/alpacon-common-action)

Run [Alpacon](https://alpacon.io) CLI commands from GitHub Actions with a token limited to the calls the workflow needs. Commands that need an interactive session, such as opening a tunnel, don't work with a service token.

Use it for work the exec and cp actions don't cover, such as listing servers or groups. The runner signs in with a service token whose scopes decide which Alpacon API calls it can make, so a token with only `server:read` cannot change anything. Alpacon is a server access platform: its agent on each server connects out to Alpacon, so no inbound port has to be open for the runner.

- [Official documentation](https://docs.alpacax.com/reference/actions/common/)

> Upgrading from `@v1`: change `alpacax/alpacon-common-action@v1` to `alpacax/alpacon-common-action@v2` and rename the `api-token` input to `service-token`. v2 accepts only a service token.

## How access is controlled

- **Scopes decide the calls**—The token's scopes, capped by its application's roles, decide which Alpacon API calls it can make. A token that can run commands or transfer files waits for approval in the Alpacon web console and authenticates nowhere until it is approved.
- **Deny by default**—For commands and file transfers, the token's access rules name the servers, commands and file paths it may use. Anything not listed is refused.
- **No interactive or admin access**—A service token cannot open a terminal session, a tunnel, or the browser editor, manage tokens or its own access rules, or approve requests.
- **Bounded**—Every token has an expiry date and a rate limit, and you can disable it at any time. You can also limit it to hours and days of the week, and, on runners with fixed addresses such as self-hosted runners, to IP ranges.
- **On the record**—Changes the token makes through the API appear on its Activity tab; reads such as listing servers do not. The commands it runs appear in the workspace audit log.

## Authentication

Set `service-token` to an Alpacon service token (its key starts with `alpst-`). A service token belongs to an IAM Application (a machine identity), not to a person, so it keeps working when people leave. A service token does not need a Work Session; endpoint permissions still apply.

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

The list shows only the servers in the token's server access rules.

### List groups

```yaml
- name: List groups
  uses: alpacax/alpacon-common-action@v2
  with:
    workspace-url: ${{ secrets.ALPACON_WORKSPACE_URL }}
    service-token: ${{ secrets.ALPACON_SERVICE_TOKEN }}
    command: "group ls"
```

This needs the `group:read` scope.

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
| `Login failed` | Wrong workspace URL or token; the token is expired, disabled or still waiting for approval; or the runner is outside the token's allowed IP ranges or hours | Check both secrets, and the token's status and restrictions in the web console |
| `permission denied: missing scope …` | The token's scopes don't cover the call | Add the missing scope to the token. The change waits for approval, and the application's roles must allow it |
| `server ls` shows no servers | The token has no server access rules | Add the servers on the token's access rules tab |
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
