---
name: Exorails — connect an agent
description: How an Exorails environment works from the agent's side: one URL, tool name prefixes, secrets you never see, what an error from the gateway means and what to tell the user.
---

# Exorails — connect an agent

You are talking to an Exorails environment: one URL that serves the tools of several servers, with every credential kept in a vault on the gateway. You never see a key, a token or a connection string, and you must never ask the user for one to "make a tool work": the gateway injects it on every call.

## Tool names

- With more than one server in the environment, tool names carry the server's short name and two underscores: `prod-db__query`, `github__create_issue`. With a single server, the bare name.
- Procedures like this one are tools prefixed `skill__`. They return instructions, they never act. Read one before starting a task it describes.

## What the gateway's messages mean

Every error from the gateway starts with `Exorails:`.

| Message | Meaning | What to do |
|---|---|---|
| `… could not connect` | the server or database is unreachable from the gateway | say so; the user checks the address, the allow-list or the Exorails Connect machine |
| `… needs authorization` | an OAuth grant expired or was revoked | ask the user to reconnect the server in the environment's Tools tab |
| `… did not answer within …` | the call timed out | retry once with a narrower request; then report |
| `monthly call limit reached` | the plan's quota is used | stop calling tools; tell the user to raise the cap or upgrade |
| `This environment requires OAuth sign-in` | the token was sent the wrong way | the user reconnects the client with sign-in |

Never paste an address or a token you happen to see into a reply: the gateway strips them, and there is nothing to gain.

## Working well

- Prefer read tools first. Write tools say so in their description and are hidden on read-only servers.
- Results are bounded (rows, hits, bytes). If a result looks cut, narrow the query rather than asking for more.
- Calls are logged by tool name and status; the user can see what you did. Say what you are about to do when it writes.
