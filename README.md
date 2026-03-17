# rec2gif

Convert macOS screen recordings to optimized GIFs for GitHub PRs.

> **One command. 30s recording → ~3MB GIF. GitHub-ready.**

```
$ rec2gif
Found: ~/Downloads/Screen Recording 2026-03-17.mov (12.4MB)
Converting...
  → Generating palette...
  → Creating GIF...
  → Optimizing...

Done! Screen Recording 2026-03-17.gif (2.8MB)
✓ GitHub ready (under 10MB)

![demo](Screen Recording 2026-03-17.gif)
```

## Install

```bash
npm install -g rec2gif
```

Requires [ffmpeg](https://ffmpeg.org/) and [gifsicle](https://www.lcdf.org/gifsicle/):

```bash
brew install ffmpeg gifsicle
```

### Agent Skill

Install as an [agent skill](https://skills.sh) so your AI coding agent knows how to use rec2gif:

```bash
npx skills add sei0/rec2gif
```

## Usage

```bash
rec2gif                          # Auto-detect latest screen recording
rec2gif recording.mov            # Convert specific file
rec2gif -w 480 -f 10 --copy      # Custom settings + copy path to clipboard
```

When multiple screen recordings are found, you'll be prompted to select one:

```
Found 5 recordings:

  1) 화면 기록 2026-03-17 오후 5.39.23.mov (375KB, 2026-03-17 17:39)
  2) Screen Recording 2026-03-15 at 2.30.00 PM.mov (1.2MB, 2026-03-15 14:30)
  3) ScreenRecording_01-08-2026 14-19-32_1.mov (10.6MB, 2026-01-08 14:19)

Select [1]:
```

## Options

| Flag | Default | Description |
|------|---------|-------------|
| `-w, --width` | 960 | Output width in pixels |
| `-f, --fps` | 15 | Frames per second |
| `-l, --lossy` | 30 | Lossy compression level (0-200) |
| `-o, --output` | auto | Output file path |
| `-d, --max-duration` | 60 | Max video duration in seconds (hard cap: 120) |
| `--no-optimize` | | Skip gifsicle optimization |
| `--copy` | | Copy Markdown snippet to clipboard |

## How it works

1. **Palette generation** — ffmpeg analyzes the video to create an optimal 256-color palette
2. **GIF creation** — ffmpeg converts using the palette with `dither=none` (best for screen recordings with crisp UI text)
3. **Optimization** — gifsicle applies `-O3 --lossy` compression

Output targets GitHub's 10MB image limit. Default settings (960px, 15fps) produce ~1-5MB GIFs for typical 3-10 second clips.

## Auto-detection

Searches `~/Downloads` for files matching known screen recording patterns:

- **English**: `Screen Recording *.mov/mp4`
- **Korean**: `화면 기록 *.mov/mp4`
- **Japanese**: `画面収録 *.mov/mp4`
- **Chinese**: `录屏 *.mov/mp4`, `螢幕錄製 *.mov/mp4`, `螢幕錄影 *.mov/mp4`
- **iPhone**: `ScreenRecording_*.mov`

If no known patterns match, falls back to showing recent `.mov/.mp4` files.

## License

MIT
