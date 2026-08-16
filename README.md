# omarchy-hotspot

A small interactive CLI to start and manage a Wi-Fi hotspot on **Omarchy** using `create_ap`.

It helps you:
- detect network and wireless interfaces
- choose internet source and hotspot adapter interactively
- enter SSID/password with validation
- show a live dashboard once the AP is enabled
- generate and display a QR code for quick phone connection
- clean up stale AP interfaces/processes on startup and exit

## Requirements

- Omarchy system with wireless hardware that supports AP mode
- `sudo` access
- Omarchy package tooling (`omarchy`) available in `PATH`

Runtime dependencies used by the app:
- `hostapd`
- `dnsmasq`
- `imv`
- `create_ap`
- `iw`

The program can prompt to install missing dependencies with:
- `omarchy pkg add ...`
- `omarchy pkg aur add ...`

This project is intended for Omarchy environments (not a general cross-distro Linux hotspot tool).

## Install prebuilt binary (no Rust needed)

You can download and use the prebuilt `v1.0.0` binary directly from the release page:

- https://github.com/cdhananjay/omarchy-hotspot/releases/tag/v1.0.0

```bash
curl -L -o omarchy-hotspot https://github.com/cdhananjay/omarchy-hotspot/releases/download/v1.0.0/omarchy-hotspot
chmod +x omarchy-hotspot
sudo install -m 755 omarchy-hotspot /usr/local/bin/omarchy-hotspot
sudo omarchy-hotspot
```

This path is recommended on Omarchy systems where Rust/Cargo is not installed.

## Build

```bash
cargo build --release
```

## Run

```bash
cargo run --release
```

## Uninstall

If you installed the prebuilt binary to `/usr/local/bin`:

```bash
sudo rm -f /usr/local/bin/omarchy-hotspot
```

If you only used a local downloaded file, remove it from the folder where you saved it:

```bash
rm -f ./omarchy-hotspot
```

If you built from source and want to remove build artifacts:

```bash
cargo clean
```

During startup, the app will:
1. check/install dependencies
2. optionally patch legacy `/usr/bin/create_ap` behavior
3. clean old virtual AP interfaces
4. prompt for hotspot settings and interface selection
5. launch `create_ap` via `sudo`

Press `Ctrl+C` to stop the hotspot and trigger cleanup.

## Notes

- Wi-Fi interface selection is restricted to detected wireless interfaces.
- QR code image is written to `/tmp/omarchy_hotspot_qr.png` and opened with `imv` when available.
- If `create_ap` exits unexpectedly, the app reports it and proceeds with cleanup.
