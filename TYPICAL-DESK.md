# Typical Desk

This fork of [RustDesk](https://github.com/rustdesk/rustdesk) is the remote-desktop engine inside Typical Compute. It is RustDesk with three changes on the `typical-desk` branch:

- **`src/common.rs`, `typical_desk_defaults()`**: the app is named `TypicalDesk`, talks only to the Typical Desk server (`desk-id.typglow.com` / `desk-relay.typglow.com`, WebSocket through Cloudflare, our server key), and hides its own tray icon and server settings. The Typical Compute controller and app drive it.
- **`src/platform/windows.rs`, `get_silent_install_options()`**: a silent install adds no desktop icon, Start Menu entry or printer.
- **`.github/workflows/typical-desk.yml`**: builds only Windows x64 and macOS arm64, unsigned, as workflow artifacts.

RustDesk is licensed under AGPL-3.0, and so is this fork. The server-side patch is in `TypicalEveningPictures/tep-hub` under `infra/typical-desk/`.
