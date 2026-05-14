# niri-tomic-os &nbsp; [![bluebuild build badge](https://github.com/tdesaules/niri-tomic-os/actions/workflows/build.yml/badge.svg)](https://github.com/tdesaules/niri-tomic-os/actions/workflows/build.yml)

See the [BlueBuild docs](https://blue-build.org/how-to/setup/) for quick setup instructions for setting up your own repository based on this template.

After setup, it is recommended you update this README to describe your custom image.

## Installation

> [!WARNING]  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/tdesaules/niri-tomic-os:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/tdesaules/niri-tomic-os:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

## ISO

![Dynamic JSON Badge](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fgist.githubusercontent.com%2Ftdesaules%2F93883cb28c830015a10172604676af73%2Fraw%2Fniri-tomic-os.uuid.json&query=%24.uuid&label=uuid&color=blue)

```bash
oras pull ttl.sh/niri-tomic-os-x86_64-$(curl -s https://gist.githubusercontent.com/tdesaules/93883cb28c830015a10172604676af73/raw/niri-tomic-os.uuid.json | jq -r '.uuid'):24h
oras manifest fetch ttl.sh/niri-tomic-os-x86_64-$(curl -s https://gist.githubusercontent.com/tdesaules/93883cb28c830015a10172604676af73/raw/niri-tomic-os.uuid.json | jq -r '.uuid'):24h
cosign verify-blob \
  --bundle niri-tomic-os-x86_64.iso.sigstore.json \
  --certificate-identity "https://github.com/tdesaules/niri-tomic-os/.github/workflows/build-iso.yml@refs/heads/main" \
  --certificate-oidc-issuer "https://token.actions.githubusercontent.com" \
  niri-tomic-os-x86_64.iso
```

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/tdesaules/niri-tomic-os
```
