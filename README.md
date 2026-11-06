<!--
SPDX-FileCopyrightText: 2026 roolrz
SPDX-License-Identifier: Apache-2.0
-->

# umount

Managed-filesystem unmount and explicit failed-provider recovery client.

Sends one target to fs-backend through the `lib-service` filesystem control protocol.
The manager synchronizes the worker and detaches the mount after its ownership checks.

See the [implementation entry](src/main.rs) and [component declaration](component.json)
for the public interfaces, build dependencies and delivered artifacts.

## Usage

In a Native session with the required capabilities:

```sh
umount --help
```

## Boundaries

Ordinary unmount returns Busy for open files, directory capabilities or nested
mounts; retry after releasing those users. `umount --force TARGET` only accepts an
already failed provider. It discards unsynchronized VFS data, reports CRITICAL,
and retires the failed worker and its queue before permitting remount. A failed
retirement leaves the volume unavailable. Healthy providers, the namespace root
and nested mounts are refused; lazy unmount is not implemented.

## Build and test

Use this checkout inside the [repo-managed workspace](https://github.com/HypeR-Proj/HypeR).
From this repository directory:

```sh
make
make test
```

An absolute `OUT=/path/to/out` selects a shared output directory. The module
entry delegates to build/system, which prepares the installed SDK and declared
dependencies. Native binaries require HypeR; host module checks cover the tests
selected by component.json. A standalone clone does not contain the workspace’s
other source repositories or Native SDK.
