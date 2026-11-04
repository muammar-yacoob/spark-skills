---
name: quickpeek-demo-videos
description: Turn a live URL into a narrated product demo video — drives a real browser through the app, writes narration from what is on the page, and renders it with voiceover and burned-in captions. Use when someone needs a demo video, a product walkthrough, a landing-page video, screenshots of a flow, or asks how to show their app working without filming it by hand.
---

# QuickPeek: demo videos from a URL

Give it a URL and it drives a real browser through the app, then renders the
recording with narration and captions. Nothing is filmed by hand and no screen
capture is involved, so the output is independent of whatever monitor the
author happens to have.

## Setup

QuickPeek runs as a local MCP server. Install it, then point your agent at it:

```json
{
  "mcpServers": {
    "quickpeek": {
      "command": "npx",
      "args": ["-y", "--ignore-scripts", "--package=@spark-apps/quickpeek", "quickpeek-mcp"]
    }
  }
}
```

`check_account` confirms the connection and what the current plan allows.

## Making one

`generate_demo` with a URL is the whole happy path. `generate_demo_from_script`
takes a step list when the flow needs a specific route through the app rather
than whatever the crawler finds.

`list_voices` before choosing one; `get_demo_config` shows what a previous run
used, which is the quickest way to reproduce a look.

## Things that cost a re-render if missed

**A bare `&` in a caption breaks speech synthesis.** It is read as the start of
an SSML entity, so that one step renders silent while every other step narrates
normally. Write "and". If a finished video has a silent step, count the audio
clips against the captioned steps before blaming the voice.

**Check what the outro claims about pricing.** The template can assert a free
tier by default. For a demo of someone else's product, or any product without
one, that is a fabricated claim sitting at the end of the video.

**A sign-in needs a fresh session.** A saved browser session goes stale quietly:
the recording still reports itself as signed in while capturing black skeleton
screens. If a run comes back empty-looking, re-authenticate rather than
re-rendering the same session.
