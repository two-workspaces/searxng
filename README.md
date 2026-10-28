# Deploy and Host SearXNG on Railway

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new/template/searxng-search?referralCode=sLjDXb&utm_medium=integration&utm_source=button&utm_campaign=searxng-search)

[SearXNG](https://github.com/searxng/searxng) is a free, privacy-respecting metasearch engine. It queries Google, Bing, DuckDuckGo, Brave, Wikipedia and 70+ other engines, merges the results and never profiles you. This template runs the official image with the **JSON API switched on**, so Open WebUI, n8n, LangChain, Perplexica and your own scripts can use it as a web-search tool right away.

## About Hosting SearXNG

One service, no database and no volume. The official `searxng/searxng` image runs pinned to a dated release. A generated secret signs cookies and image-proxy URLs. The base URL follows your Railway domain. On boot, the start command adds `json` to the allowed result formats; everything else is upstream's defaults. It idles at about 100 MB, so it fits the smallest plan.

## Common Use Cases

- Web search for AI agents and chat UIs: Open WebUI, LibreChat, n8n, Flowise, LangChain, Perplexica
- A private search engine for yourself or your team, with no tracking and no ads
- A search API for scripts and scrapers: one HTTP call, results from many engines as JSON

## Dependencies for SearXNG Hosting

- None. Add a Valkey service only if you open the instance to the public and want SearXNG's bot limiter.

### Deployment Dependencies

- [SearXNG on GitHub](https://github.com/searxng/searxng)
- [SearXNG documentation](https://docs.searxng.org/)

### Implementation Details

**Search:** open your Railway domain.

**JSON API:** `https://<your-domain>/search?q=railway&format=json`. Use these settings in your app:

- **Open WebUI:** Admin Settings → Web Search → engine `searxng`, query URL `https://<your-domain>/search?q=<query>`.
- **n8n / LangChain:** point the SearXNG tool at `https://<your-domain>`.

**Settings** (in the service's Variables tab):

- `SEARXNG_BASE_URL`: change it if you add a custom domain.
- `GRANIAN_WORKERS`: raise it on bigger plans for more parallel searches.
- `SEARXNG_LIMITER`: turn the bot limiter on. This needs a Valkey service with `SEARXNG_VALKEY_URL=redis://<valkey-host>:6379/0`. Note that the limiter also blocks most API clients.

**Upgrading:** change the image tag in Settings → Source to a newer dated tag from [GitHub packages](https://github.com/searxng/searxng/pkgs/container/searxng) and redeploy.

Notes and limits:

- The instance is open to anyone who knows the URL. Keep the URL to yourself, or put the limiter on if you share it.
- Some engines (DuckDuckGo, Google) occasionally answer datacenter IPs with a CAPTCHA. SearXNG skips them for a while and the other engines fill in. Disable chronic offenders in Preferences → Engines.

## Why Deploy SearXNG on Railway?

Railway is a singular platform to deploy your infrastructure stack. Railway will host your infrastructure so you don't have to deal with configuration, while allowing you to vertically and horizontally scale it.

By deploying SearXNG on Railway, you are one step closer to supporting a complete full-stack application with minimal burden. Host your servers, databases, AI agents, and more on Railway.
