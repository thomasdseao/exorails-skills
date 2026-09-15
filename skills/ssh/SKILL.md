---
name: "SSH guide"
description: "Using the Exorails SSH connector well: what run, read_file, list_files and system_info do, how the command allow-list works, why some commands are refused, and how to work within it."
---

# SSH

Tools: `system_info`, `list_files`, `read_file`, `run`. The gateway holds the key or password; you never see it.

## Start with system_info

It reports the OS, the kernel, the CPU count, memory and disk, and the current user. Read it before assuming a package manager or a path.

## Reading

- `list_files` lists a directory with sizes and modification times. `read_file` returns a file's content, bounded in size; ask for a range or grep through `run` when a file is large.
- Logs: `run` with `tail -n 200 /var/log/…` or `journalctl -u service -n 200 --no-pager`.

## Running commands

- The user may have set an allow-list of commands. When it exists, only those commands run, and a command carrying shell operators (`|`, `;`, `&&`, redirections, subshells) is refused, because a pipe would let anything past the list. Run one plain command at a time.
- Destructive patterns are refused regardless: `rm -rf` on root paths, `mkfs`, `dd` to a device, `shutdown`, fork bombs. Do not try to rephrase them.
- Output is bounded. Prefer `head`, `tail`, `grep` and `wc` to dumping a whole file.
- Long-running commands time out. Use `timeout 20 …` yourself when a command might hang.

## Do not

- Do not modify `~/.ssh`, sudoers, or system services unless the user asked for exactly that and confirmed.
- Do not read files that look like secrets (`.env`, private keys) unless the task requires it; if you must, say what you read and never quote a secret in a reply.
