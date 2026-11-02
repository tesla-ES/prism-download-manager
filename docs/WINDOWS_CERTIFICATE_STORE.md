# Windows certificate store for yt-dlp

Prism 1.2.4 keeps `--ignore-config` on every yt-dlp call. On Windows, the new **Use Windows Certificate Store** switch under Settings > Video & Streams also passes `--compat-options no-certifi`. It is enabled by default on Windows and disabled by default elsewhere. The switch is saved in Prism's normal settings document and applies to both format probing and yt-dlp downloads.

`no-certifi` makes yt-dlp use the operating system's trusted certificate chain instead of its bundled certifi bundle. TLS certificate verification remains active. This helps when an approved network proxy's certificate chain is trusted by Windows but is absent from certifi. It does not alter Windows trust settings, the proxy setting, browser cookies, FFmpeg, Deno, or Prism's direct HTTP download engine.

If the network's certificate is not trusted by Windows, this setting will not make that connection succeed. Prism's managed yt-dlp needs to support `--compat-options no-certifi`.

Verification: `crates/prism-video/tests/plan.rs` checks enabled and disabled arguments, including the existing tool, proxy, and cookie arguments. `crates/prism-core/src/settings.rs` checks the default and persisted switch value.
