# Kitesurf prebuilt Worker

Download and deploy the compiled Kitesurf Worker without running Bazel or building the source yourself.

## Download

Get **[kitesurf-worker-bundle-2026-11-17.zip](https://github.com/aur0/kitesurf-worker-build/releases/download/prebuilt-2026-11-17/kitesurf-worker-bundle-2026-11-17.zip)** from the [release page](https://github.com/aur0/kitesurf-worker-build/releases). The ZIP contains the already-compiled Worker, its assets, Wrangler configuration, and license notices. It does not contain an API key.

This build omits the bundled Chrome DevTools visual frontend. The browser engine still supports CDP clients such as Puppeteer and Playwright, including page navigation, HTML retrieval, and screenshots.

## Deploy the prebuilt bundle

You need a Cloudflare account on the Workers Paid plan with Dynamic Workers enabled, plus Node.js and Wrangler. These steps deploy the compiled files as-is; they do not run the Kitesurf build.

1. Unzip the download.
2. In a terminal, change into the extracted `worker` directory.
3. Log in to Cloudflare if needed, then deploy:

   ```powershell
   npx wrangler login
   npx wrangler deploy --config wrangler.json
   ```

4. Set an API key for your Worker. Wrangler prompts for the value; use a long, randomly generated key and keep it private:

   ```powershell
   npx wrangler secret put KITESURF_API_KEY --name kitesurf
   ```

   The Worker fails closed with `503` until the secret is set. It returns `401` when a request has no key or the wrong key. If you changed the Worker name in `wrangler.json`, pass that name to `--name` instead.

5. Use the `workers.dev` hostname Wrangler reports for your Worker. Send `Authorization: Bearer <your-key>` on HTTP and WebSocket requests. Keep the key in your application's secret store; do not put it in source code, a URL, or a public repository.

Each person deploying the bundle uses their own Cloudflare account and their own API key. This release does not provide access to anyone else's running Worker.

## Connect with Puppeteer

Install `puppeteer-core` in your client project and set `KITESURF_API_KEY` in that client's environment:

```js
import puppeteer from "puppeteer-core";

const browser = await puppeteer.connect({
  browserWSEndpoint: "wss://<worker-host>/devtools/browser",
  headers: {
    Authorization: `Bearer ${process.env.KITESURF_API_KEY}`,
  },
});

const page = await browser.newPage();
await page.goto("https://example.com/");
const html = await page.content();
await page.screenshot({ path: "page.png" });
await browser.disconnect();
```

Replace `<worker-host>` with the hostname from your own deployment. Discovery endpoints are `/json/version` and `/json/list`. Playwright can connect over CDP using the same authorization header.

## License and source

This prebuilt bundle is based on [cloudflare/kitesurf](https://github.com/cloudflare/kitesurf) and is licensed under Apache-2.0. See the [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE) files.