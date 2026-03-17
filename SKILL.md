---
name: rec2gif
description: Convert macOS screen recordings to optimized GIFs. Use when user asks to create a GIF from a screen recording, convert video to GIF, or generate a demo GIF.
---

# rec2gif

Convert macOS screen recordings to optimized GIFs for GitHub PRs.

## Prerequisites

- macOS only
- `ffmpeg` and `gifsicle` must be installed: `brew install ffmpeg gifsicle`
- `rec2gif` CLI must be installed: `npm install -g rec2gif`

If any of these are missing, guide the user to install them before proceeding.

## When to Use

- User asks to convert a screen recording to GIF
- User wants to create a demo GIF from a `.mov` or `.mp4` file
- User needs an optimized GIF (under 10MB) for GitHub PRs or documentation

## Usage

### Basic: Convert the latest screen recording

```bash
rec2gif
```

This auto-detects the most recent screen recording in `~/Downloads`.

### Convert a specific file

```bash
rec2gif path/to/recording.mov
```

### With custom settings

```bash
rec2gif -w 480 -f 10 --copy recording.mov
```

### All options

| Flag | Default | Description |
|------|---------|-------------|
| `-w, --width` | 960 | Output width in pixels |
| `-f, --fps` | 15 | Frames per second |
| `-l, --lossy` | 30 | Lossy compression level (0-200) |
| `-o, --output` | auto | Output file path |
| `-d, --max-duration` | 60 | Max video duration in seconds (hard cap: 120) |
| `--no-optimize` | | Skip gifsicle optimization |
| `--copy` | | Copy output file path to clipboard |

## Optimization Tips

If the output GIF exceeds GitHub's 10MB limit:

1. Reduce width: `-w 480` (default: 960)
2. Lower FPS: `-f 10` (default: 15)
3. Increase lossy compression: `-l 80` (default: 30)

## How It Works

1. **Palette generation** — ffmpeg analyzes the video to create an optimal 256-color palette
2. **GIF creation** — ffmpeg converts using the palette with `dither=none` (best for screen recordings with crisp UI text)
3. **Optimization** — gifsicle applies `-O3 --lossy` compression

Output is placed next to the input file with a `.gif` extension by default.
