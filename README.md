# Page Factory

A single static HTML file that turns a few lines of brief into a complete, self-contained landing page. Bring your own model — llama.cpp, Ollama, LM Studio, vLLM, OpenAI, OpenRouter, Groq, or Anthropic.

No backend, no build step, no dependencies. Your key and your text go to the endpoint you name and nowhere else.

## How it works

**Stage 1** turns your notes into a real design brief: content type, a style chosen mechanically from four content-appropriate candidates, palette hexes, a Google Fonts pairing, a type scale, an ASCII wireframe, page architecture derived from your content, a signature interactive module, the full copy deck, a motion spec, and a lead-capture spec.

**Stage 2** builds that brief into one `index.html`.

The brief stays editable between stages. That's the point — rewrite the parts you disagree with before you spend tokens on a build.

## Run it

Drop `index.html` anywhere. GitHub Pages: push to a repo, Settings → Pages → deploy from branch root.

For local models, serve it over plain http so the browser doesn't block the request:

```
python3 -m http.server 4000
# then open http://localhost:4000
```

## Endpoints

| Runtime | Base URL | Key |
|---|---|---|
| llama.cpp `llama-server` | `http://localhost:8080/v1` | blank |
| Ollama | `http://localhost:11434/v1` | blank |
| LM Studio | `http://localhost:1234/v1` | blank |
| vLLM | `http://localhost:8000/v1` | blank |
| OpenAI | `https://api.openai.com/v1` | `sk-…` |
| OpenRouter | `https://openrouter.ai/api/v1` | `sk-or-…` |
| Anthropic | `https://api.anthropic.com/v1` | `sk-ant-…` (switch format to Anthropic) |

Press **Fetch** to list models the endpoint actually offers.

## Two things that will bite you

**Mixed content.** A page served over HTTPS cannot call `http://localhost`. Serve this file over http locally, or put TLS in front of your server. This is browser policy, not a bug here.

**CORS.** Start `llama-server` with `--host 0.0.0.0`; it sends permissive headers by default. Ollama needs `OLLAMA_ORIGINS=*`.

The app tells you which one you hit when a request fails.

## Notes

- Set **max tokens** to at least 16000. A full page runs long, and a truncated build is the most common failure. The status line flags it when `</html>` never arrives.
- Temperature around 0.9 for Stage 1, lower if you want Stage 2 more literal. One control covers both; run stages separately if you want different values.
- Your key is only stored in `localStorage` if you tick the box, and only in your own browser.
- The prompts live in the **PROMPTS** panel. Edit them. That's the actual product — the UI is just a runner.

## License

MIT. Do what you like with it.
