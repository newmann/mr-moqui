# Patched crates of the desktop app

`[patch.crates-io]` in `../Cargo.toml` replaces these crates for the desktop app only (its own workspace). Each is the
crate's published source, with the changes below marked `MR Moqui patch` in the code.

## download-cef 3.0.0

The first build downloads the CEF binary distribution. `download-cef` 3.0.0 loads the catalog
(`https://cef-builds.spotifycdn.com/index.json`) with ureq's `read_json`, which refuses a body over 10 MB, and the
catalog is larger, so the download never starts. `CefIndex::download_from` raises that limit to 128 MB. The archive
itself is streamed and was not the failure.

`./moq desktop` stops when `cargo tree -i download-cef` shows a download-cef that is not this copy (cargo builds the
crates.io crate the patch no longer matches with only a warning), so a `cef-dll-sys` update cannot drop the patch unseen.

To move to a newer download-cef: copy its published source here, apply the same limit, and check that a clean build
gets past the catalog fetch.
