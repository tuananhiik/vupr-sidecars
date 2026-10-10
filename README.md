# vupr-sidecars

Release assets only: sidecar binaries for the VUPR dub engine, built by `tools/sidecars/build.sh` in the VUPR repository. Each asset's sha256 is pinned in the app; a rebuild ships under a new tag.

## Releases

| Tag | Target | Built by |
|---|---|---|
| `dub-2026.10.10` | `aarch64-apple-darwin` | `tools/sidecars/build.sh` on a Mac, uploaded by hand |
| `dub-win-2026.10.10` | `x86_64-pc-windows-msvc` | `.github/workflows/windows.yml` (`workflow_dispatch`, input `tag`) |

**macOS** (`dub-2026.10.10`): one static binary per program (`espeak-ng`, `omnivoice-tts`, `omnivoice-codec` with Metal, `vupr-dub-sherpa`), `espeak-ng-data.tar`, `SHA256SUMS`, and licences.

**Windows** (`dub-win-…`): every asset is named `<file>-x86_64-pc-windows-msvc.<ext>` and installs under its name minus that suffix, all in one directory, because the executables load DLLs by their real names.

- espeak-ng 1.52.0: `espeak-ng.exe` and `libespeak-ng.dll` repackaged from the official `espeak-ng.msi`. There is no Windows data archive; every platform uses `espeak-ng-data.tar` from `dub-2026.10.10`.
- `msvcp140`, `vcruntime140`, `vcruntime140_1`: the Visual C++ runtime, app-local.
- omnivoice.cpp: `omnivoice-tts.exe` and `omnivoice-codec.exe` with ggml as DLLs (`ggml`, `ggml-base`, one `ggml-cpu-*` per CPU level, `ggml-cuda`). Backends load at runtime, so without NVIDIA's `cudart64_12`, `cublas64_12` and `cublasLt64_12` (shipped separately, see `LICENSE-cuda-redist`) it falls back to CPU.
- vupr-dub-sherpa: one static exe, built from the private `tts` repo. The workflow needs the `TTS_READ_TOKEN` secret for it and skips it without the secret.
- `SHA256SUMS-windows` and `LICENSE-*` files.

Both releases are immutable: the app pins each asset's sha256, so a rebuild ships under a new tag.
