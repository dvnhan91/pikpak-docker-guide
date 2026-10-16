# PikPak CLI for Docker

Use the official PikPak CLI in Docker on Linux, Unraid, and Dockhand, with persistent login and local file transfers.

Independent community project.

## AI agent skill

```bash
npx skills add dvnhan91/pikpak-docker-guide --skill pikpak-docker
```

[View on skills.sh](https://www.skills.sh/dvnhan91/pikpak-docker-guide/pikpak-docker)

Ask your agent to:

- Set up PikPak in Docker or Dockhand.
- Save URLs or magnets and track cloud tasks.
- Upload files or download them to your NAS.
- Obtain playback URLs.
- Troubleshoot login, paths, and permissions.

Example: “Set up PikPak CLI in Dockhand with persistent login.”

Your agent needs terminal or SSH access to operate the Docker host.

## Quick start

Download this repository, then run:

```bash
docker compose up -d
docker compose logs -f pikpak-cli
```

After installation finishes, exit the log viewer with Ctrl+C:

```bash
docker compose exec pikpak-cli pikpak auth login
docker compose exec pikpak-cli pikpak ls
```

Follow the displayed browser authorization instructions.

## Dockhand / Unraid

Paste `compose.yaml` into a Dockhand stack. On Unraid, use persistent host paths:

```yaml
    volumes:
      - /mnt/user/appdata/pikpak-cli/config:/root/.pikpak
      - /mnt/user/pikpak-files:/files
```

Files under `/files` inside the container appear in the mapped host folder. Keep both mappings when recreating the container.

## Optional referral

The author may earn commission on qualifying Premium purchases through invitation code **56485809**.

New account with this code:

```bash
docker compose exec pikpak-cli pikpak auth register --affiliate 56485809
```

Without a referral:

```bash
docker compose exec pikpak-cli pikpak auth register --affiliate=
```

Existing users should use `auth login`.

## Status

Manual CLI installation, browser login, and cloud listing were tested on Unraid AMD64 with CLI v0.5.3. Skill installation was also tested.

Automatic Compose setup, ARM64, transfers, cloud tasks, and playback workflows still need end-to-end testing.

Keep credentials and the `config` directory private.

[Official PikPak CLI](https://mypikpak.com/en-US/cli)
