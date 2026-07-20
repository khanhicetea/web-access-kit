# web-access-kit

A pi package that adds two web access tools:

- **`web_fetch_page`** — read a normal public webpage as compact Markdown (uses `curl` + `node-html-markdown`; not a general curl replacement).
- **`web_search`** — search current web data with Antigravity CLI (`agy`) in headless mode, using its Google Search capability.

It also bundles the `web-access-kit` skill with a source-first research workflow.

## Requirements

- [pi](https://pi.dev)
- `curl` on `PATH`
- For `web_search`: an installed and authenticated `agy` executable on `PATH`

Confirm the commands are available:

```bash
curl --version
agy --help
```

## Install

From this checkout:

```bash
pi install ./web-access-kit
```

Or test without installing:

```bash
pi -e ./web-access-kit
```

After editing an installed local package, run `/reload` in pi.

## Usage

Ask pi naturally:

```text
Search the web for the latest stable Node.js release and cite official sources.
```

```text
Read https://example.com as a webpage and summarize it.
```

You can force-load the bundled workflow with:

```text
/skill:web-access-kit research the latest release of Bun
```

To select only these extension tools in print mode:

```bash
pi -e ./web-access-kit --tools web_search,web_fetch_page -p \
  "Find today's official Node.js release information and cite sources"
```

## Behavior and safety

- `web_fetch_page` accepts only HTTP and HTTPS, follows redirects, limits downloads to 5 MB, converts HTML responses to compact Markdown with `node-html-markdown`, and limits model-visible output to pi's standard 2,000-line/50-KB cap. Use it for readable webpage content; use shell `curl` for APIs, binaries, auth, or raw responses.
- `web_search` runs `agy --model 'Gemini 3.5 Flash (Low)' --sandbox --mode plan --print ...` and limits model-visible output to the same cap.
- Full truncated output and binary downloads are placed in temporary files and their paths are returned.
- Do not include credentials in URLs. Tool arguments and results can be retained in pi sessions.
- Web content is untrusted and may contain prompt injection; the bundled search prompt and skill tell agents not to follow page instructions.

## Development

Validate package contents:

```bash
npm pack --dry-run
```

Test extension loading without making a model request:

```bash
pi -e ./web-access-kit --list-models >/dev/null
```

## License

MIT
