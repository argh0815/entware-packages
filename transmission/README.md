Transmission 4.1.3 for Entware x64-3.2
=======================================

Target
------
Architecture: x64-3.2
glibc:        2.27
Compiler:     GCC 14.3.0
Transmission: 4.1.3-1

This build uses a private GCC 14 runtime for Transmission under:

    /opt/lib/transmission-4.1/

It does NOT replace Entware's global libstdc++, libgcc_s or libatomic.

Exact source revisions
----------------------
Entware:
969c703e6fd8b2ad84d82affaeb14b48d1fcb105

Entware packages feed:
b6a6f2962f62882b76dfe45f9f9e1238cd9b74fd

Files
-----
entware.patch
    Main Entware changes required for GCC 14:
    - glibc 2.27 build compatibility using -fcommon
    - gettext-full compatibility using -std=gnu23

packages.patch
    Transmission package changes:
    - update Transmission to 4.1.3
    - remove obsolete miniupnpc compatibility patch
    - add glibc 2.27 compatibility patch
    - add private GCC runtime package
    - use private Transmission RPATH
    - package libstdc++, libgcc_s and libatomic privately

entware.config
    Build configuration used for this build.

REVISIONS.txt
    Exact source revisions used.

Applying the patches
--------------------
From the Entware source directory:

    git checkout 969c703e6fd8b2ad84d82affaeb14b48d1fcb105

    git -C feeds/packages checkout b6a6f2962f62882b76dfe45f9f9e1238cd9b74fd

    git apply /path/to/entware.patch

    git -C feeds/packages apply /path/to/packages.patch

Copy the supplied configuration:

    cp /path/to/entware.config .config

    make defconfig

Build the toolchain:

    make toolchain/install -j$(nproc) V=s

Build Transmission:

    make package/feeds/packages/transmission/compile -j1 V=s

Output packages
---------------
Packages are created under:

    bin/targets/x64-3.2/generic-glibc/packages/

Expected packages:

    transmission-runtime_4.1.3-1_x64-3.2.ipk
    transmission-daemon_4.1.3-1_x64-3.2.ipk
    transmission-cli_4.1.3-1_x64-3.2.ipk
    transmission-remote_4.1.3-1_x64-3.2.ipk
    transmission-web_4.1.3-1_all.ipk

Runtime design
--------------
Transmission binaries use:

    RPATH=/opt/lib/transmission-4.1

The private runtime contains:

    libstdc++.so.6
    libgcc_s.so.1
    libatomic.so.1

Normal Entware libraries continue to load from /opt/lib.

This allows Transmission 4.1.3 to use the newer GCC 14 C++ runtime
without replacing Entware's global GCC runtime libraries.