<!--
SPDX-FileCopyrightText: 2026 roolrz
SPDX-License-Identifier: Apache-2.0
-->

# umount

Graceful managed-filesystem unmount client.

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

Open files, directory capabilities and nested mounts can return Busy. Forced and lazy
detach are deliberately unsupported; retry after releasing users of the mount.

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
