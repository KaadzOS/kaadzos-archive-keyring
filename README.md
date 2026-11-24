<div align="center">
  <img src="https://raw.githubusercontent.com/KaadzOS/.github/main/assets/kaadzoslogo.png" alt="KaadzOS" width="100%">
</div>

## KaadzOS archive keyring

This repository publishes the public signing key used by the optional KaadzOS APT archive and by signed release checksums. It contains public material only; the private signing key is never stored here.

**Fingerprint**

`7FC3 1B27 E931 570B 2D8C 3E42 9A4F 8D9E 9D5B 0C7A`

| File | Use |
| --- | --- |
| [`kaadzos-archive-keyring.asc`](kaadzos-archive-keyring.asc) | ASCII-armored key for inspection and manual import. |
| [`kaadzos-archive-keyring.gpg`](kaadzos-archive-keyring.gpg) | Binary keyring for APT's `Signed-By`. |

### Inspect the key

```sh
gpg --show-keys --with-fingerprint kaadzos-archive-keyring.asc
```

The fingerprint above must match exactly before you trust the key.

### Use the APT archive

```sh
sudo install -d /usr/share/keyrings
curl -fsSL https://iso.kaadzos.pages.dev/apt/kaadzos-archive-keyring.asc |
  sudo gpg --dearmor -o /usr/share/keyrings/kaadzos-archive-keyring.gpg

echo 'deb [signed-by=/usr/share/keyrings/kaadzos-archive-keyring.gpg] https://iso.kaadzos.pages.dev/apt trixie main' |
  sudo tee /etc/apt/sources.list.d/kaadzos.list

sudo apt update
```

The archive is optional. Removing the source file and keyring restores the stock Debian-only setup. Full instructions and troubleshooting live in [`docs/apt-repository.md`](https://github.com/KaadzOS/KaadzOS/blob/main/docs/apt-repository.md).

### Verify a release

The same key signs the release checksum. Keep the keyring beside the ISO when verifying:

```sh
gpg --import kaadzos-archive-keyring.asc
gpg --verify KaadzOS-trixie-amd64.iso.sha256.asc \
  KaadzOS-trixie-amd64.iso.sha256
sha256sum --check KaadzOS-trixie-amd64.iso.sha256
```

If the key ever rotates, the fingerprint in this README and the published keyring change together. Check the [latest release](https://github.com/KaadzOS/KaadzOS/releases/latest) or the [main repository](https://github.com/KaadzOS/KaadzOS) before trusting a new key.

<sub>Debian is a registered trademark of Software in the Public Interest, Inc. KaadzOS is an independent project.</sub>
