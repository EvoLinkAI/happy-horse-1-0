# Happy Horse 1.0

English | [Español](README.es.md) | [Português](README.pt.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Français](README.fr.md) | [Türkçe](README.tr.md) | [繁體中文](README.zh-TW.md) | [简体中文](README.zh-CN.md) | [Русский](README.ru.md)

[![Happy Horse 1.0 Banner](banner.png)](https://evolink.ai/happy-horse?utm_source=github_readme_en&utm_medium=banner&utm_campaign=happy-horse)

Track the latest Happy Horse 1.0 signals in one place — API integration guides, multi-model benchmark comparisons, runnable scripts, and community reaction across X and Reddit.

Happy Horse 1.0 is trending because it broke into top AI video benchmark discussions, `@HappyHorseATH` appeared as the official X account, and Alibaba Group publicly posted about it. At the same time, there is still no confirmed official website, official domain, or official try-it URL.

[Join Early Access](https://evolink.ai/happy-horse?utm_source=github_readme_en&utm_medium=cta&utm_campaign=happy-horse)

## Table of Contents

- [API Quick Start](#api-quick-start)
- [Model Comparison Overview](#model-comparison-overview)
- [Video Sample Gallery](#video-sample-gallery)
- [Creation Guide](#creation-guide)
- [Running the Scripts](#running-the-scripts)
- [Trending Context & Community Signals](#trending-context--community-signals)
- [Happy Horse 1.0 Benchmarks](#happy-horse-10-benchmarks)
- [FAQ](#faq)
- [Disclaimer](#disclaimer)

## API Quick Start

Happy Horse 1.0 API is live via [Evolink](https://evolink.ai). Get your API key from the [API Key Management Page](https://evolink.ai/dashboard/keys).

Text-to-Video — single curl call, async task:

```bash
curl --request POST \
  --url https://api.evolink.ai/v1/videos/generations \
  --header 'Authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
  "model": "happyhorse-1.0-text-to-video",
  "prompt": "A horse galloping through a sunlit meadow, golden hour lighting, cinematic wide shot",
  "quality": "720p",
  "aspect_ratio": "16:9",
  "duration": 5,
  "seed": 42
}'
```

The response returns a task ID — poll it to get the video URL (valid 24h).

Key parameters:

| Parameter | Options | Default |
|-----------|---------|---------|
| `quality` | `720p`, `1080p` | `720p` |
| `aspect_ratio` | `16:9`, `9:16`, `1:1`, `4:3`, `3:4` | `16:9` |
| `duration` | `3`–`15` seconds | `5` |

Prompt tip: shot-by-shot descriptions yield better multi-shot results — e.g. `Shot 1 [0~3s] wide angle: ...; Shot 2 [3~6s] medium shot: ...`

Full API docs: [Text-to-Video](https://docs.evolink.ai/en/api-manual/video-series/happyhorse1.0/happyhorse-1.0-text-to-video) · [Image-to-Video](https://docs.evolink.ai/en/api-manual/video-series/happyhorse1.0/happyhorse-1.0-image-to-video)

## Model Comparison Overview

How does Happy Horse stack up against other leading AI video generation models?

| Model | Provider | T2V | I2V | Audio | Max Res | FPS | $/sec | API | Open Weights |
|-------|----------|:---:|:---:|:-----:|---------|:---:|------:|:---:|:------------:|
| Happy Horse 1.0 | Alibaba ATH | Yes | Yes | Yes | 1080p | 24 | TBD | Yes | Unconfirmed |
| Seedance 2.0 | ByteDance | Yes | Yes | No | 1080p | 24 | $0.08 | Yes | No |
| Kling 1.6 | Kuaishou | Yes | Yes | No | 1080p | 30 | $0.07 | Yes | No |
| Runway Gen-4 | Runway | Yes | Yes | No | 1080p | 24 | $0.05 | Yes | No |
| Pika 2.2 | Pika | Yes | Yes | No | 1080p | 24 | $0.06 | Yes | No |

T2V = text-to-video, I2V = image-to-video. Full structured data: [`benchmarks/models.json`](benchmarks/models.json)

## Video Sample Gallery

We use a set of [standardized prompts](samples/prompts.json) to compare output quality across models. Each prompt tests a specific capability:

| Prompt | Tests | Difficulty |
|--------|-------|:----------:|
| White horse galloping through meadow | Motion quality, lighting | Medium |
| Cat swimming underwater | Water physics, refraction | Hard |
| City intersection dusk-to-night | Scene complexity, light trails | Hard |
| Portrait with head turn and smile | Facial expressions, subtle motion | Hard |
| Coffee being poured with steam | Fluid dynamics, light interaction | Medium |

*Side-by-side comparison GIFs will be added as models are tested. See [`samples/`](samples/) for details.*

## Creation Guide

Happy Horse 1.0 supports four core workflows. Choose the mode based on the assets you already have:

| You have... | Best mode | Inputs |
|-------------|-----------|--------|
| Just an idea | Text-to-Video | Text prompt only |
| One image you want to animate | First-frame Image-to-Video | 1 image + optional prompt |
| Multiple images for style, identity, or storyboard control | Reference Image-to-Video | 1-9 images + required prompt |
| An existing clip you want to modify | Video Editing | 1 video + optional reference images + prompt |

### Prompt Formula

A strong Happy Horse prompt usually follows this structure:

`Scene + Subject + Motion + Audio`

- **Scene**: where the action happens, including environment, lighting, and atmosphere
- **Subject**: the person, object, or character the video focuses on
- **Motion**: character movement, camera movement, and transitions
- **Audio**: spoken lines, ambient sound, or sound effects when needed

Example:

```text
[Scene] A sunlit Paris cafe in late afternoon.
[Subject] A man in a navy suit sits across from a woman in a crimson dress.
[Motion] He leans forward while she slowly stirs her coffee; the camera pushes in gently.
[Audio] He says, "You knew from the beginning, didn't you?" over soft cafe ambience.
```

### First-frame vs Reference Image-to-Video

| Mode | Image role | Image count | Prompt | Output framing |
|------|------------|-------------|--------|----------------|
| First-frame I2V | The uploaded image becomes frame 1 and is closely preserved | 1 | Optional, but recommended | Follows the source image |
| Reference I2V | Images act as visual references for identity, style, or story beats | 1-9 | Required | Freely selectable: `16:9`, `9:16`, `1:1`, `4:3`, `3:4` |

Use **first-frame mode** when the opening shot must match your source image exactly. Use **reference mode** when you want the model to borrow appearance, scene, or storyboard cues without locking the full composition to a single first frame.

### Input Specs by Mode

| Mode | Key input rules | Output rules |
|------|-----------------|--------------|
| Text-to-Video | Prompt required, up to 5000 English chars or 2500 Chinese chars | `720p`/`1080p`, `3-15s`, aspect ratio selectable |
| First-frame I2V | 1 image, `.jpg`/`.jpeg`/`.png`/`.webp`, up to 30MB, short side >= 300px | `720p`/`1080p`, `3-15s`, framing follows the input image |
| Reference I2V | 1-9 images, same image limits as above, prompt required | `720p`/`1080p`, `3-15s`, aspect ratio selectable |
| Video Editing | 1 `.mp4`/`.mov` H264 video up to 100MB, optional 1-4 reference images | `720p`/`1080p`, `3-15s`, framing follows the source video |

### Practical Tips

#### Text-to-Video

- Be concrete instead of abstract: describe subject, action, scene, camera language, and mood.
- Match duration to complexity: `3-5s` for simple motion, `8-15s` for multi-step scenes.
- Shot-by-shot prompts work well for narrative clips.

#### First-frame Image-to-Video

- Use a clean, high-quality image with a clear, unobstructed subject.
- Focus the prompt on what happens after the still frame starts moving.
- Pick images that already imply motion for better continuation.

#### Reference Image-to-Video

- Keep references visually consistent and close to the target aspect ratio.
- Use prompt references like `Image 1`, `Image 2`, etc. when combining multiple assets.
- Best for character consistency, style transfer, and storyboard-driven generation.

#### Video Editing

- State both what to change and what must stay unchanged.
- Use reference images only when they directly support the intended edit.
- Big style shifts may require matching lighting or audio instructions to avoid mismatch.

### Common Use Cases

- **Short drama**: character consistency, emotional close-ups, cinematic lighting
- **E-commerce ads**: product demos, digital spokesperson videos, scalable creative production
- **Social content**: stylized short clips, memes, product seeding, image-to-video remixing

Full platform walkthrough, prompt examples, and Chinese usage guide: [`happyhorse-master/guide.md`](happyhorse-master/guide.md)

## Running the Scripts

### Requirements

- Python 3.10+
- API keys for the models you want to use

### Setup

```bash
cd scripts
pip install -r requirements.txt
cp .env.example .env
# Edit .env with your API keys — you only need keys for the models you plan to use
```

### Commands

```bash
python generate.py --list                    # list all supported models
python generate.py --model happyhorse-1.0 -p "prompt here"   # generate from one model
python compare.py --dry-run -m happyhorse-1.0,seedance-2.0 -p "prompt"  # preview costs
python compare.py -m happyhorse-1.0,seedance-2.0,kling-1.6 -p "prompt"   # run comparison
```

See [`scripts/README.md`](scripts/README.md) for full documentation and how to add new models.

## Trending Context & Community Signals

For background on why Happy Horse is trending and detailed social media signal tracking:

- [Trending Context](docs/trending-context.md) — Latest 24h update, source map, why it's trending, current status
- [Community Signals](docs/community-signals.md) — Signal snapshot, X/Twitter analysis, Reddit discussion

## Happy Horse 1.0 Benchmarks

The benchmark angle is the main reason this keyword is hot.

What readers should understand:

- People are not discussing Happy Horse because of a polished launch.
- They are discussing it because it appeared to perform unusually well in a respected public comparison context.
- That benchmark signal then caused speculation, reposting, reverse-engineering, and unofficial SEO pages.

This means benchmark visibility created the demand before official clarity existed.

## FAQ

### Is this an official Happy Horse repository?

No. This is a public intelligence hub built from social and community signals.

### Is Happy Horse 1.0 open source?

Open-source claims are widespread, but the strongest public signal points to API access via Evolink, not confirmed open weights.

### Is there an official website or official try-it link?

No confirmed one is publicly available right now. Claims about an official website, official domain, or official try-it URL should currently be treated as false or unofficial.

### Is Happy Horse really better than Seedance 2.0?

The public conversation says it is competitive enough to trigger real attention. It does not say that every serious user agrees it is clearly better in every scenario.

### Why are so many people talking about Alibaba?

Because the story moved from rumor to partial confirmation: Artificial Analysis tied Happy Horse to Alibaba's ATH AI Innovation Unit, `@HappyHorseATH` appeared as the official X account, and Alibaba Group posted a public recognition message.

### Why does Reddit matter if X is bigger?

Because Reddit exposes skepticism, open-weights interest, and tooling intent more clearly than the raw X hype cycle.

### How do I use the comparison scripts?

See [Running the Scripts](#running-the-scripts) above. You need Python 3.10+ and API keys for the models you want to test. The `--dry-run` flag lets you preview what will be called and estimated costs before making any API requests.

### Which model should I use?

It depends on your priorities. Happy Horse leads on benchmarks and is the only model with native audio — its API is live via [Evolink](https://evolink.ai). Runway Gen-4 offers the lowest per-second pricing, Kling 1.6 has the highest FPS, and Seedance 2.0 is strongest on physical consistency.

## Disclaimer

This repository is not an official Happy Horse project. It aggregates public discussion from X/Twitter and Reddit for research, monitoring, and discovery. Public claims can change quickly. Treat rumored technical details, release dates, attribution claims, website claims, and try-it links as provisional unless they are directly confirmed through a clearly official public channel.

The comparison scripts call third-party APIs that may incur costs. Use the `--dry-run` flag to preview estimated costs before running. API endpoints, pricing, and availability are based on publicly available information and may change without notice.
