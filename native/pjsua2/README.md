# native/pjsua2

`pjsua2.i` and `symbols.i` are from pjproject 2.17 (GPLv2-or-later; the tarball's
SHA-256 is recorded in `pjproject-2.17.tar.gz.sha256`). ai-phone is AGPLv3, which
GPLv3 §13 permits for GPLv2-or-later code. The compiled extension ai-phone uses is
built exactly by `build.sh` (`scripts/install.sh` runs it and copies the result into
`src/ai_phone/vendor/`); this directory is the corresponding source (GPLv3 §6(d)).
PJSIP's license text is included in [COPYING](COPYING).

Build on macOS: `brew install swig pkgconf pjproject`, then `./build.sh`. Outputs land in
`build/` (git-ignored). `smoke.py` starts PJSIP headless with the null audio device.

The build resolves library search paths from the installed pjproject's Homebrew
runtime dependencies, including its OpenSSL version. Hard-coding a different
OpenSSL major can produce an extension that compiles but fails to import with a
missing symbol (for example, `_OPENSSL_sk_set_cmp_thunks`). If an older build fails
this way, rerun `./scripts/install.sh` from the repository root to rebuild it.

`pjsua2.i` also defines two ai-phone helpers, `ai_phone_set_keepalive(tcp_sec, tls_sec)` and
`ai_phone_get_keepalive()`, exposing PJSIP's process-wide TCP/TLS CRLF keepalive interval (`pjsip_cfg()`),
which pjsua2 does not expose.
