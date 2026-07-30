# csp-builder

Build Content-Security-Policy headers visually. Pick directives, toggle keywords, add custom hosts, copy as a header / meta tag / Express snippet. Single HTML file. Browser-only.

**Live demo:** https://0xelitesystem.github.io/csp-builder/

## Why

CSP is one of the most useful security headers and one of the most painful to write correctly. The MDN reference is dense, the keywords are easy to typo (forgetting the quotes around `'self'`), and getting the directive list right is its own thing.

This tool gives you a checklist UI: pick which directives you want, toggle the keywords each one supports, paste in your custom hosts. The output is generated live and warns about common pitfalls.

## Use it

Open `index.html` in any browser, or visit the hosted demo at `https://0xelitesystem.github.io/csp-builder/` once Pages is enabled.

1. Five sensible defaults are pre-checked: `default-src 'self'`, `script-src 'self'`, `style-src 'self'`, `img-src 'self'`, `connect-src 'self'`.
2. Tweak each directive: toggle keywords, paste extra hosts (space-separated).
3. Switch tabs in the output panel: HTTP Header, HTML meta tag, or Express.js middleware snippet.
4. Copy.

## Directives covered

| Directive | What it controls |
|---|---|
| `default-src` | Fallback for all `*-src` directives |
| `script-src` | Where JavaScript can load from |
| `style-src` | Where stylesheets can load from |
| `img-src` | Where images can load from |
| `font-src` | Where fonts can load from |
| `connect-src` | Where fetch/XHR/WebSocket can connect to |
| `media-src` | Where audio/video can load from |
| `frame-src` | Where iframes can load from |
| `frame-ancestors` | Who can iframe THIS page (modern X-Frame-Options) |
| `object-src` | `<object>` and `<embed>` sources (almost always `'none'`) |
| `base-uri` | What `<base href>` can be set to |
| `form-action` | Where forms can submit |

## Built-in warnings

The tool flags common mistakes:

- `'unsafe-inline'` defeats CSP for that directive, use nonces or hashes
- `'unsafe-eval'` is high-risk, only some libraries actually need it
- Wildcard `*` on the effective script source (`script-src`, or `default-src` when `script-src` is absent)
- `data:` on the effective script source, which is close to `'unsafe-inline'`
- `'none'` listed next to other sources, which browsers ignore because `'none'` must be the only source
- `'strict-dynamic'` with no nonce or hash, which blocks every script tag on the page
- `frame-ancestors` in the HTML meta output, which browsers ignore outside a real header
- Missing `default-src` AND `script-src` (browsers may default to permissive)
- Missing `frame-ancestors` (clickjacking risk)

## Strong recommendation

Always deploy a new CSP using `Content-Security-Policy-Report-Only` first. Watch the violation reports for at least a week. Tighten as needed. Only switch to enforcing mode (`Content-Security-Policy`) once the report stream is clean.

If you don't have a report endpoint, use a service like [report-uri.com](https://report-uri.com) or set up a cheap Cloudflare Worker to log them.

## Tech

- Single HTML file, ~370 lines
- Vanilla JS, no frameworks, no dependencies
- Light and dark themes with OS preference detection
- WCAG AA contrast on both themes

## What it doesn't do

- Doesn't generate nonces or hashes. Those need to be generated server-side per request.
- Doesn't validate against a real test page. Use the browser DevTools "Issues" tab or `report-uri.com` for that.
- Doesn't support every CSP directive. The ones not listed (`worker-src`, `manifest-src`, `prefetch-src`, etc.) are rarely needed; add them manually if you do.
- Doesn't generate Trusted Types policies.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. See [LICENSE](LICENSE).

## Related

- [webhook-inspector](https://github.com/0xelitesystem/webhook-inspector), local webhook receiver via Service Worker
- [cron-builder](https://github.com/0xelitesystem/cron-builder), visual cron expression builder
- [single-file-saas-template](https://github.com/0xelitesystem/single-file-saas-template), ship a SaaS in one HTML file
