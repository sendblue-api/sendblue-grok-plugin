# Sendblue

Send customer messages, read replies, and check delivery from your Sendblue workspace in Claude, Grok Bot, and Cursor.

The plugin connects to Sendblue's hosted MCP server using Streamable HTTP and OAuth. Sign in with your Sendblue account, choose the workspace, and grant the permissions you want to use.

## Connect in Claude

1. Open **Customize > Connectors** and select **Add > Add custom connector**.
2. Enter **Sendblue** as the name and `https://api.sendblue.co/mcp` as the server URL.
3. Review the detected OAuth configuration. Use **Register automatically** for the OAuth client and leave request headers empty.
4. Complete setup, connect, and sign in to Sendblue. Select the workspace and approve the read and/or send permissions you need.
5. Start a conversation with Sendblue enabled and ask Claude to check the connection.

Team and Enterprise owners add the connector in their organization settings first. Members then connect their own Sendblue accounts from **Customize > Connectors**.

The custom connector is available now. The Claude directory listing is being prepared for review.

## Connect in Grok Bot

1. Open **Connect apps** and choose **Add a custom connector**.
2. Enter **Sendblue** as the name and select **Remote HTTPS**.
3. Enter `https://api.sendblue.co/mcp` as the server URL.
4. Leave HTTP headers and Client ID empty. Choose **Add plugin**.
5. Complete the browser authorization. If the connector shows an initial error, choose **Reconnect** and complete authorization.
6. Return to Grok Bot and verify **Connected** with **6 of 6 enabled**.

This is the custom-connector installation flow. Marketplace distribution is pending review.

## Connect in Cursor

Add this server to your MCP configuration, then complete its OAuth sign-in.

```json
{
  "mcpServers": {
    "sendblue": {
      "url": "https://api.sendblue.co/mcp"
    }
  }
}
```

For local plugin development, copy this plugin directory into `~/.cursor/plugins/local/sendblue`, reload Cursor, and inspect the discovered components in Customize. Follow your team's plugin policy.

## Available tools

| Tool | Purpose |
| --- | --- |
| `ping` | Check connectivity and echo a supplied message |
| `list_lines` | Find sending numbers assigned to the connected account |
| `list_messages` | Read recent messages, with contact and direction filters |
| `send_message` | Send one text message from the connected account |
| `get_message_status` | Check a message using the UUID returned by Sendblue |
| `evaluate_service` | Summarize recent outbound messaging health over a bounded window |

## Example requests

- Check my Sendblue connection and list my available sending numbers.
- Show the three most recent messages for the contact I specify.
- Send the exact text I provide to the recipient I specify, then check its status.
- Summarize outbound messaging health over the past 24 hours, including sampling and denominators.

## Permissions and sending

`messages:read` permits message-history and account-health reads. `messages:send` permits message sending. Send-only access can check the status of a send made by the connected app. Access follows the workspace and permissions authorized in Sendblue.

Use a fresh UUID `idempotency_key` for each intentional send. Retry the same send with identical inputs and the same key. Check the returned message UUID when the outcome is uncertain.

Free-plan accounts must specify a sending line and use a recipient whose shared-line routing is already established. Follow the Sendblue dashboard's recipient setup flow before testing sends on such accounts.

`evaluate_service` samples at most the newest 100 messages in the requested window. Include the window, sample size, total count, and relevant denominators when reporting its results. SMS lacks delivery receipts, so confirmed-delivery rates need to be interpreted alongside the service mix.

Disconnect or revoke access through Sendblue Connected Apps. Grok Bot connections are account-wide and may be used by the other Bots on that account.

## Support and terms

- [Sendblue documentation](https://docs.sendblue.com/)
- [Sendblue website](https://www.sendblue.com/)
- Support email is support@sendblue.com.
- [Privacy policy](https://www.sendblue.com/privacy-policy)
- [Terms of service](https://www.sendblue.com/terms-of-service)

## Platform references

- [Claude custom connectors](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp)
- [Grok Bot app connections](https://docs.x.ai/grok-bot/computer-and-apps)
- [Cursor MCP configuration](https://cursor.com/docs/mcp)
- [Cursor plugin packaging](https://cursor.com/docs/reference/plugins)
