# MT8188 SCP firmware (staging)

Firmware blob and the pending linux-firmware patch for the MT8188
(Geralt / Ciri) SCP, staged here for review before submission to
[linux-firmware](https://gitlab.com/kernel-firmware/linux-firmware).

## Contents

```
mediatek/mt8188/scp_c0.img    firmware blob, same path as in linux-firmware
patches/*.patch               git binary patch for linux-firmware (main)
```

## Firmware

| | |
|---|---|
| version string | `geralt_scp_v2.0.30090-ca800f117 private-mt-scp:v0.0.221-54db2d4` |
| size | 636548 bytes |
| md5 | `6f34baa4fdc15668902a42b0c0186f49` |
| load addresses | 0x10500000 (SRAM), 0x50000000 (DRAM) |

This replaces `v2.0.27748` in linux-firmware, which was built from a
modified private tree (hence the `+` marker in its version string).

## Provenance

Built from clean ChromiumOS firmware sources:

```
ec-legacy        ca800f1179  (v2.0.0-30090-gca800f1179)
private-mt-scp   54db2d4     (v0.0.221)

make BOARD=geralt_scp -j16
```

Built with the MediaTek private tree (`mt8188/`) absent, so the MDP part
comes from the committed prebuilt `board/geralt_scp/lib/libmt_scp.a`.
This is how the ChromeOS release builder builds this image. Both source
repos report a clean tree, and the version timestamp comes from the git
commit rather than the build date, so the build is reproducible.

## Applying the patch

```sh
git clone https://gitlab.com/kernel-firmware/linux-firmware.git
cd linux-firmware
git am /path/to/patches/0001-mediatek-mt8188-update-SCP-firmware-to-v2.0.30090.patch
```

## Status

Blob verified only structurally so far (ELF load layout matches the
previous releases). Hardware verification -- decoding against a software
reference on a Geralt DUT -- is pending.
