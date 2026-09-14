# cosmic-zainium

COSMIC desktop recipes for Zainium OS. Same `ZEXBUILD` + `manifest.toml`
format as `syshub-recipes`, `userland-recipes`.
`ci/build.sh`/`ci/changed-packages.sh` are copied unmodified from there.

Upstream source: `pop-os/cosmic-epoch` (mirrored at `~/cosmic-epoch`),
tracking its `epoch-*` tags per component.

## Status

## Independent service stack

- `org.freedesktop.login1` (session tracking, power, brightness, polkit
  agent identity) → `quantra-logind`.
- All D-Bus traffic → `oxibus-daemon`. It also binds the legacy
  `/run/dbus/system_bus_socket` path so unmodified `libdbus` binaries
  (polkit, PipeWire, xdg-desktop-portal) still work.
- PAM → `elevate-pam`. Not a `libpam.so.0` shim — `cosmic-greeter` is
  source-patched to depend on the `elevate-pam` crate directly and call
  its Rust API from `locker.rs`. Password auth only; session open/close
  is quantra-logind's job now, no PAM session module involved.
- `cosmic-settings` builds with a real `QuantraServiceManager` (new,
  patched in) instead of the stock `openrc` feature — real OpenRC
  tools don't exist on Zainium either, so that feature would've just
  silently no-op'd. Shells out to `quantra-ctl` instead.
- No `.service`/`.target` files get installed anywhere.

Everything installs under `--prefix=/overlayer/syshub`, not `/usr`.


## Packages

| Package | Notes |
|---|---|
| `cosmic-applets` | |
| `cosmic-applibrary` | |
| `cosmic-bg` | |
| `cosmic-comp` | Zainium branding resources |
| `cosmic-edit` | |
| `cosmic-files` | |
| `cosmic-greeter` | elevate-pam integration, quantra service units |
| `cosmic-icons` | |
| `cosmic-idle` | |
| `cosmic-initial-setup` | path patch |
| `cosmic-launcher` | |
| `cosmic-monitor` | path patch |
| `cosmic-notifications` | |
| `cosmic-osd` | path patch |
| `cosmic-panel` | |
| `cosmic-player` | |
| `cosmic-randr` | |
| `cosmic-screenshot` | |
| `cosmic-session` | path patch |
| `cosmic-settings` | real QuantraServiceManager (quantra-ctl) |
| `cosmic-settings-daemon` | path patch |
| `cosmic-sound-theme` | |
| `cosmic-wallpapers` | |
| `cosmic-workspaces-epoch` | |
| `pop-launcher` | path patch |
| `xdg-desktop-portal-cosmic` | path patch |
