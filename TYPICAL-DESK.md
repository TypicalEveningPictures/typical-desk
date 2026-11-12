# Typical Desk

This fork of [RustDesk](https://github.com/rustdesk/rustdesk) is the remote-desktop engine inside Typical Compute. It is RustDesk with these changes on the `typical-desk` branch:

- **`src/common.rs`, `typical_desk_defaults()`**: the app is named `TypicalDesk`, talks only to the Typical Desk server (`desk-id.typglow.com` / `desk-relay.typglow.com`, WebSocket through Cloudflare, our server key), and hides its own tray icon and server settings, and listens for direct connections on the local network (`direct-server`, TCP 21128, because 21118 is the ID server's websocket port and a machine that also hosts the server can't listen there) so machines on the same network or tailnet skip the relay. The Typical Compute controller and app drive it.
- **`src/core_main.rs`, `typical_desk_cli_allowed()`**: on macOS, `--password` and `--option` work for the signed-in user. A Mac hosts as that user (a LaunchAgent the controller installs runs `--server`), with no root service, and the controller runs as the same user. At `--server` start it asks once for Screen Recording and Accessibility.
- **`src/tray.rs`**: the hidden tray stays hidden on macOS too.
- **`src/platform/windows.rs`, `get_silent_install_options()`**: a silent install adds no desktop icon, Start Menu entry or printer.
- **Icons**: every RustDesk icon (`res/*.png`, `res/icon.ico`, the Windows runner's `app_icon.ico`, the Mac `AppIcon.icns`) is the Typical Desk icon, Typical Projects' design reading "Typical Desk". Its source is `res/typical-desk-icon/AppIcon.icon` (Icon Composer), rendered with Icon Composer's `ictool`.
- **`.github/workflows/typical-desk.yml`**: builds only Windows x64 and macOS arm64, unsigned, as workflow artifacts. The Windows build runs on a self-hosted runner on TypicalPC (`C:\actions-runner`, label `typical-win`, service `actions.runner.TypicalEveningPictures-typical-desk.typicalpc`); GitHub's runner took about 45 minutes. macOS builds on GitHub's runner until a Mac with Xcode and room to spare is set up. The Mac app is then signed with the Developer ID before it is published, so its macOS permissions survive engine updates.

RustDesk is licensed under AGPL-3.0, and so is this fork. The server-side patch is in `TypicalEveningPictures/tep-hub` under `infra/typical-desk/`.
