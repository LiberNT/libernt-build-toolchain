# LiberNT Build Toolchain

Prebuilt Windows toolchain (RosBE) for building LiberNT, published as GitHub
releases. This repository holds only the binaries and this README; it has no
build sources.

Latest release: [**v20261015**](https://github.com/LiberNT/libernt-build-toolchain/releases/tag/v20261015)

## Downloads

| File | What it is |
|------|------------|
| `rosbe.exe` | Bootstrapper: downloads, verifies, installs and updates the toolchain |
| `rosbe-<version>-win-x64.zip` | Full toolchain bundle (~2 GB) |
| `rosbe-bootstrapper-<version>-win-x64.zip` | `rosbe.exe` packaged as a ZIP |
| `SHA256SUMS.txt` | SHA-256 checksums for the files above |

## Install on Windows

Download `rosbe.exe` from the
[latest release](https://github.com/LiberNT/libernt-build-toolchain/releases/latest),
then in PowerShell, from the folder that holds it:

```powershell
setx ROSBE_RELEASE_REPO LiberNT/libernt-build-toolchain
setx ROSBE_DISABLE_ATTESTATION 1
$env:ROSBE_RELEASE_REPO = "LiberNT/libernt-build-toolchain"
$env:ROSBE_DISABLE_ATTESTATION = "1"

.\rosbe.exe install
.\rosbe.exe enable
```

`ROSBE_RELEASE_REPO` points the bootstrapper at this repository instead of its
built-in default. `ROSBE_DISABLE_ATTESTATION` is needed because the GitHub
artifact attestations were recorded for the original build repository, not
this one. The bundle is still checked against `SHA256SUMS.txt` before it is
installed under `%LOCALAPPDATA%\RosBE`.

`setx` keeps both variables for new terminals so later `status` and `update`
runs keep working; the `$env:` lines apply them to the current one.

`rosbe enable` adds the toolchain to your user `PATH`, not `rosbe.exe` itself,
so keep `rosbe.exe` in a fixed folder and run it from there:

```powershell
.\rosbe.exe status
.\rosbe.exe update
.\rosbe.exe disable
.\rosbe.exe remove
```

### Verify a download by hand

```powershell
Get-FileHash .\rosbe-20261015-win-x64.zip -Algorithm SHA256
```

Compare the hash with the matching line in `SHA256SUMS.txt`.

## What's in v20261015

| Component | Version |
|-----------|---------|
| LLVM-MinGW (Clang + lld + libc++) | 20261007 |
| LLVM-MinGW RISC-V (`llvm-mingw-riscv24`, not on `PATH`) | 20261007 |
| LLVM (`llvm`, every LiberNT Clang target, not on `PATH`) | 20261015 |
| MinGW-GCC (crosstool-NG Canadian-cross, UCRT) | 16.2.0 |
| CMake | 3.31.6 |
| Ninja | 1.12.1 |
| Flex + Bison (winflexbison) | 2.5.25 |
| QEMU | 11.1.0 |

The LLVM toolchains are built from
[sjljeh/llvm-project](https://github.com/sjljeh/llvm-project) (branch
`libernt`). Targets: `i686`, `x86_64` and `aarch64` Windows (UCRT); the
`llvm` toolchain also covers ARM, PowerPC and RISC-V.

## License

MIT for the bootstrapper. Bundled third-party toolchains keep their own
licenses.
