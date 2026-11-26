# pi-remote

> [!IMPORTANT]
> **Active development has moved to [llMooreyll/my-pi-extensions](https://github.com/llMooreyll/my-pi-extensions).**
>
> This repository is an early standalone snapshot (last updated 2026-11-09) and is no longer maintained here. The latest version of `pi-remote`—including newer mobile UI features, host daemon improvements, and integration with companion extensions such as `pi-trigger`—is actively developed in:
>
> 👉 **[llMooreyll/my-pi-extensions/tree/main/extensions/pi-remote](https://github.com/llMooreyll/my-pi-extensions/tree/main/extensions/pi-remote)**
>
> （本仓库为早期独立快照，已停止同步更新；`pi-remote` 的最新代码与完整功能均在 **[llMooreyll/my-pi-extensions](https://github.com/llMooreyll/my-pi-extensions/tree/main/extensions/pi-remote)** 中持续维护。）

---

Use the [pi coding agent](https://www.npmjs.com/package/@earendil-works/pi-coding-agent) running on your Linux computer from an Android phone: watch sessions live, send prompts, steer or stop runs, answer dialogs, transfer files both ways, and use `/model`, `/thinking`, `/tree`, `/new`, `/fork`, `/resume`, `/compact`, and `/settings`. You can also inspect subagents and team members.

You host the whole network yourself. A small relay on your own VPS only forwards end-to-end encrypted frames: it stores nothing and cannot read anything. The computer needs no public IP. To move to another VPS, computer, or phone, you change an address or re-pair; nothing else.

## How it fits together

```text
Android app ──ws(s)──▶ relay on your VPS ◀──ws── pi-hostd on the computer ◀──unix socket── pi + bridge extension
             (or directly over the LAN)            (systemd user service)                  (every pi TUI in tmux)
        └──────────── Noise NK end-to-end encryption between app and hostd ────────────┘
```

## Components

| Path | Runs on | What it is |
|---|---|---|
| `packages/hostd/` | Linux computer | `pi-hostd`: pairing, device auth, process registry, event replay, file transfers, starts pi in tmux, session list, and settings. |
| `packages/relay/` | Linux VPS | `pi-relay`: stateless WebSocket relay splicing app and hostd connections (with Dockerfile and systemd unit). |
| `packages/bridge/` | inside pi | `pi-remote-bridge`: the pi extension streaming events, running remote ops, mirroring dialogs, and reporting subagent/team roles. |
| `apps/mobile/` | Android | Expo / React Native mobile app. |
| `packages/protocol/` | shared | Wire types, op definitions, error codes, and version constants. |

## Getting the Latest Version

Please clone and follow the up-to-date setup instructions from the main monorepo instead of this snapshot:

- Repository: https://github.com/llMooreyll/my-pi-extensions
- `pi-remote` directory: https://github.com/llMooreyll/my-pi-extensions/tree/main/extensions/pi-remote

## Security

- App ↔ hostd traffic is end-to-end encrypted with Noise NK (`@getpaseo/relay` e2ee). The phone learns the host's public key from the pairing QR code; the relay sees only ciphertext, IPs, timing, and sizes.
- The relay secret only stops strangers from using your relay. The identity file and device credentials under `~/.config/pi-remote/` must be kept private.
- Commands sent from the phone execute with your user's permissions on the computer. Revoke a lost device promptly.

## Fonts

The bundled fonts are Instrument Serif, JetBrains Mono, Space Grotesk, and GB2312 subsets of Noto Serif SC and Noto Sans SC, all under the SIL Open Font License 1.1.

## License

This project is licensed under the GNU General Public License v3.0 (GPL-3.0) — see [LICENSE](LICENSE) for details.
