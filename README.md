# RakuOS Gamescope ISO

This repository builds the live ISO for RakuOS Gamescope, the edition that
boots into Steam's Game Mode (gamescope-session) with a Plasma desktop beside
it.

It is the ISO-side companion to the `rakuos-gamescope` image repository and is
built in GitLab CI with [RakuForge](https://gitlab.com/rakuos/packages/rakuos/rakuos-forge).

## What the ISO contains

- The installed image: `quay.io/rakuos/rakuos-gamescope:<tag>`, in the ISO's
  bootc image store; the installer installs it from there.
- A live session built from that same image. The live session logs into
  Plasma, not Game Mode, so the installer has a desktop to run in: RakuForge
  masks the image's `autologin-setup` units in the live system only. The
  installed system keeps the image's own Game Mode autologin.

## Purpose

This repo keeps ISO-specific logic separate from the image repo. It owns:

- the ISO pipeline configuration
- the live ISO hooks (`config/configure-iso.sh`, `config/pre_initramfs.sh`)
- ISO naming and upload behavior

The image itself comes from the `rakuos-gamescope` image repository.

## Branches

The pipeline picks the image tag from the branch:

- `main` -> `latest` (stable)
- `staging` -> `staging`
- `rawhide` -> `rawhide`
- `rakuosel` -> `rakuosel`

## Pipeline

GitLab CI builds one ISO (`build-iso-standard`) named
`rakuos-gamescope[-<branch>]-<date>-amd64.iso`, plus a `-latest-` copy, and
stores checksums as artifacts. Optional uploads (manual pipeline inputs):

- `rakuos-server-upload`
- `sourceforge-upload`

Downloads land at `https://dl.rakuos.org/gamescope/`.

## Repo layout

- [config/configure-iso.sh](config/configure-iso.sh): live session setup
  (installer, live packages, shared and KDE live files)
- [config/pre_initramfs.sh](config/pre_initramfs.sh): live image cleanup
  before the initramfs is built
- [build-local.sh](build-local.sh): local build with the installed
  `rakuos-forge` (`./build-local.sh [branch_slug]`)
- [.gitlab-ci.yml](.gitlab-ci.yml)
