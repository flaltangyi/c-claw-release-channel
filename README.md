# C-CLAW Release Channel

This repository is the public/private release-channel skeleton for C-CLAW USB updates.

It should contain small update manifests and release metadata. Large artifacts should be hosted through OSS/CDN, a model mirror, or explicit split-file releases, then referenced by URL and SHA256.

Do not put C-CLAW source code, signing private keys, upstream API keys, customer data, or multi-GB GGUF models in this repository.

## Layout

- `channels/stable.json`: stable channel manifest consumed by customer USBs.
- `channels/beta.json`: beta/test channel manifest for internal USBs.
- `packages/.gitkeep`: placeholder for small test packages only.
- `models/.gitkeep`: placeholder for model metadata only; do not commit multi-GB GGUF files to normal Git.
- `.secrets/`: local-only signing key workspace, ignored by Git.

## Customer USB

Create `update-source.txt` in the USB root and put the manifest URL or a local manifest path in it. Then run:

```powershell
Update-CClaw.bat
```

The updater checks `release-manifest.json`, downloads a package if configured, verifies SHA256 and RSA-SHA256 signature, backs up config, and preserves `data/`, `models/`, and `local-model/`.

Hermes GGUF model upgrades use a separate model updater with resumable `.part` downloads and disk-space checks. Do not mix model upgrades into the small shell update package.
