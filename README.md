# Agent Slack app

Generic [Slack app manifest](manifest.yaml) for an agent. It turns on the Agent messaging experience (DMs), `@mentions` in channels, and Socket Mode.

The file has no tokens, no signing secret, and no request URL. Each workspace that uses it gets its own app.

The bot can join public channels (`conversations.join`), @mention people and user groups, add and read emoji reactions, and use the workspace's custom emoji. A private channel still needs a member to invite the bot. If the app is already installed, update the manifest in App Settings and reinstall so Slack grants the new scopes.

## Create it in a workspace

1. Open [Create a Slack app](https://api.slack.com/apps?new_app=1) and choose **From a manifest**.
2. Pick the workspace.
3. Open the YAML tab, paste `manifest.yaml`, then **Next** → **Create**.
4. **Install to Workspace** and allow the bot scopes.
5. **Basic Information** → **App-Level Tokens** → **Generate Token and Scopes**. Add the scope `connections:write`. Copy the `xapp-` token.
6. **OAuth & Permissions** → copy the **Bot User OAuth Token** (`xoxb-`).
7. Point the agent process at those two tokens. Socket Mode connects outbound, so the agent does not need a public URL.

Rename `display_information.name` and `features.bot_user.display_name` before pasting if this workspace already has an app called Agent.

Agent DMs need a paid Slack plan, or a [Slack Developer Program](https://api.slack.com/developer-program) sandbox.

## Send this to someone in another workspace

Yes. Send them `manifest.yaml`, or a link:

```text
https://api.slack.com/apps?new_app=1&manifest_yaml=<url-encoded YAML>
```

They follow the steps above in **their** workspace. Slack creates a new app there: new app id, new bot token, new signing secret. Their messages stay in their workspace and reach whatever agent process holds **their** `xoxb-` and `xapp-` tokens.

A workspace admin may have to approve the install if that workspace restricts custom apps.

This does not install your existing app into their workspace, and it does not attach their workspace to your running agent.

## One app, many workspaces

That is a different setup. Host one agent, set `socket_mode_enabled` to `false`, put your HTTPS event, interactivity, and OAuth redirect URLs in the manifest, then **Activate Public Distribution** and share the **Add to Slack** link. You store one bot token per workspace. Do not use the paste-this-file flow for that.
