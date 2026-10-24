# Patched eframe 0.37.0 for BeamCraft

This repository carries the exact `eframe` 0.37.0 crate as published on crates.io
(archive SHA-256 `9c5642501a65ca657d05f00c643a5c14ff2346cb3818032bfe23d58e1f8e5f84`,
verified against the checksum pinned in BeamCraft's `Cargo.lock`) with two changes:

- [Apibim-com/BeamCraft#149](https://github.com/Apibim-com/BeamCraft/issues/149):
  `data/icon.png` is deleted and the `load_default_egui_icon` fallback in
  `src/native/epi_integration.rs` is replaced by a generated opaque 16x16 placeholder,
  so the upstream default icon (author and licence not provable) is never compiled into
  a BeamCraft binary. The manifest `include` list is updated accordingly.
- [Apibim-com/BeamCraft#225](https://github.com/Apibim-com/BeamCraft/pull/225): on
  Windows, when the executable carries an icon group as resource 1, `set_app_icon_windows`
  in `src/native/app_icon.rs` sets that resource (loaded with `LR_SHARED`) as the big and
  the small window icon instead of resampling the runtime image, so the shell draws the
  exact size from the ICO (such as the 24 px taskbar icon). Without the resource the
  upstream behaviour is unchanged. The `windows-sys` feature `Win32_System_LibraryLoader`
  is added to the manifest for `GetModuleHandleW`.

Everything else is byte-identical to the published archive, including the crate's own
`Cargo.lock`. Upstream: https://github.com/emilk/egui (`crates/eframe`), licence
`MIT OR Apache-2.0`; the upstream `LICENSE-APACHE` and `LICENSE-MIT` texts (tag `0.37.0`)
are included because the crates.io archive ships without them.

Maintained by the BeamCraft contributors (Apibim). Drop this fork once upstream eframe
stops embedding an unattributed default icon (or ships one with a provable author and
licence) and BeamCraft moves to that release.
