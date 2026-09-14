---
name: Exorails Connect — private networks
description: What to do when a tool cannot reach a database or server on a private network: Exorails Connect runs an agent on the user's machine and declares routes; you cannot set it up, but you can explain it.
---

# Exorails Connect — private networks

A tool that answers `could not connect` for a private address (10.x, 192.168.x, an internal hostname) is not broken: the gateway lives on the internet and cannot reach the user's network. Exorails Connect fixes that with a small agent the user runs inside the network. It opens an outbound tunnel; nothing inbound, no port to open.

## What you can do

You cannot enrol a machine or declare a route: those are actions in the Exorails console. You can tell the user precisely what to do:

1. In the environment, open **Private network** → **Add a machine**. A one-time token and an install command appear.
2. On a machine inside the network (a bastion, a runner, a VM): run the install command, then `exorails-connect up --token …`. The machine shows as online.
3. In **Machines**, declare a route for each target: a name, a host and a port, for example `prod-db → 10.0.0.5:5432`. Nothing else is reachable through the machine.
4. Back in the tool's settings, choose **Reach via** the machine and the route.

Once done, retry the tool. Nothing changes on your side: same tool names, same calls.

## What to avoid

- Do not suggest exposing the database on the internet or adding the gateway's IP to an allow-list when a private network is involved; Exorails Connect is the intended path.
- Do not ask for SSH keys, VPN credentials or the machine's token. The user enters them in the console, never in the conversation.
