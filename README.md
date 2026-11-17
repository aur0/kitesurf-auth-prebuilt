# Kitesurf prebuilt Worker bundle

This repository distributes a compiled Kitesurf Cloudflare Worker bundle. Download the ZIP from [Releases](https://github.com/aur0/kitesurf-worker-build/releases) to deploy it without running the Bazel build.

The package includes the compiled Worker, static assets, Wrangler configuration, and deployment instructions. It omits the bundled Chrome DevTools frontend and requires a `KITESURF_API_KEY` Worker secret. No API key is included.

To run it, use your own Cloudflare account and Workers Paid plan with Dynamic Workers enabled, then follow the README inside the downloaded archive. The archive is a deployable build package, not a running browser service or a replacement for a Cloudflare account.

Based on [cloudflare/kitesurf](https://github.com/cloudflare/kitesurf) and licensed under Apache-2.0; license notices are included in the archive.