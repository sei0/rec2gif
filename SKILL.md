---
name: rec2gif
description: Convert macOS screen recordings to optimized GIFs for GitHub PRs. Use when user asks to create a GIF, convert a screen recording, make a demo GIF, turn a video into a GIF, or needs a PR-ready GIF under 10MB.
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
- User says "make a GIF", "convert to GIF", "screen recording to GIF"
- User wants to attach a visual demo to a PR

## Example Prompts

These are examples of what users might ask:

- "Convert my latest screen recording into a GIF"
- "Make a GIF from this .mov file for the PR"
- "Turn the newest screen recording in Downloads into a GIF under 10MB"
- "Create a demo GIF and copy the Markdown to clipboard"
- "Convert recording.mov to a smaller GIF at 480px width"

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

### With custom settings + copy Markdown to clipboard

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
| `--copy` | | Copy Markdown snippet to clipboard |

## Output

After conversion, rec2gif prints:
- File size and GitHub readiness check
- A ready-to-paste Markdown image snippet: `![demo](filename.gif)`
- With `--copy`: the Markdown snippet is copied to clipboard

## Optimization Tips

If the output GIF exceeds GitHub's 10MB limit:

1. Reduce width: `-w 480` (default: 960)
2. Lower FPS: `-f 10` (default: 15)
3. Increase lossy compression: `-l 80` (default: 30)

## Troubleshooting

- **"Missing dependencies: ffmpeg"** → Run `brew install ffmpeg gifsicle`
- **"No screen recordings found"** → Specify a file directly: `rec2gif path/to/file.mov`
- **GIF too large** → Use lower settings: `rec2gif -w 480 -f 10 -l 80`
- **"Command not found: rec2gif"** → Run `npm install -g rec2gif`
