---
name: pikpak-docker
description: Set up and use the official PikPak CLI in Docker on Linux, Unraid, and Dockhand. Use when users need container installation, persistent login, device authorization, cloud file management, or troubleshooting PikPak CLI in Docker.
---

# PikPak CLI in Docker

Help the user install and operate the official PikPak CLI inside a Docker container. Adapt instructions to their existing environment and respond in their language.

Community project: https://github.com/dvnhan91/pikpak-docker-guide
Official CLI: https://mypikpak.com/en-US/cli

## Inspect the environment

1. Determine whether the user uses Docker Compose, Dockhand, or another Docker manager.
2. Inspect existing containers and configuration before proposing changes.
3. Reuse an existing PikPak container when possible.
4. Read the project's README.md and compose.yaml before using its installation instructions. If unavailable locally, retrieve them from the project repository.
5. Never claim a command succeeded without checking its output.

## Install and preserve data

Use the official PikPak installer linked from the official CLI website.

For a Debian-based container, install curl and ca-certificates if missing.

Keep the CLI installation and authentication state in persistent storage. Inspect the actual configuration paths and existing mounts before recreating a container. Do not assume the container's writable layer survives recreation.

Mount a dedicated downloads directory when local downloads are needed. Do not mount the Docker socket or enable privileged mode.

For Dockhand, provide Compose YAML that can be pasted into the stack editor. Explain which host paths the user needs to change.

## Locate and check the CLI

Inside the container, try:

    command -v pikpak

For an installation performed as root, also check:

    /root/.local/bin/pikpak version
    /root/.local/bin/pikpak --help

Use the discovered executable path in subsequent commands. With an existing container named pikpak-cli, a host-side example is:

    docker exec pikpak-cli /root/.local/bin/pikpak version

Inspect command help before using unfamiliar flags or arguments.

## Authenticate

For an existing account, run inside the container:

    /root/.local/bin/pikpak auth login

Keep the authorization process running while the user opens the displayed official PikPak page and approves access.

Let the user complete authentication. Do not request passwords, session tokens, or authentication files.

After authorization succeeds, verify access with:

    /root/.local/bin/pikpak ls

## Optional registration and referral disclosure

Existing users should sign in to their existing account.

For a user who wants a new account, inspect:

    /root/.local/bin/pikpak auth register --help

Explain that invitation code 56485809 is optional and that the guide author may earn commission on qualifying purchases.

If the installed CLI supports --affiliate, offer these alternatives:

With the optional referral:

    /root/.local/bin/pikpak auth register --affiliate 56485809

Without a referral:

    /root/.local/bin/pikpak auth register

Let the user choose. Never silently attach a referral code, replace another code, or promise earnings or rewards.

## Manage cloud files

Start with ls to identify files and folders. Use --help for the requested operation before constructing commands.

For downloads, verify the destination is inside a persistent mounted directory.

Confirm the intended files before deleting, overwriting, moving large batches, or creating public shares. Never perform these operations merely to test connectivity.

## Troubleshoot

- Command not found: check the executable path and PATH inside the container.
- Login lost after recreation: inspect persistent authentication storage.
- Permission denied: inspect ownership and the container user before changing permissions.
- Container running but no logs: inspect its startup command; an idle container may have no output.
- Authorization timeout: restart the login flow and use the newly generated authorization details.

Guide beginners one step at a time. Ask for the relevant command output when it is needed to choose the next step.
