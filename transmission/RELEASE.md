# Transmission 4.1.3-1 for Entware x64-3.2

Transmission 4.1.3 built for Entware x64-3.2 using GCC 14.3.0 and glibc 2.27.

## Private GCC runtime

Transmission uses an isolated GCC runtime under:

`/opt/lib/transmission-4.1/`

containing:

- `libstdc++.so.6`
- `libgcc_s.so.1`
- `libatomic.so.1`

This does not replace Entware's global GCC runtime libraries.

## Packages

- transmission-runtime
- transmission-daemon
- transmission-cli
- transmission-remote
- transmission-web

Install `transmission-runtime` together with the Transmission packages.
