# Stickman Studio AI

Open-source deterministic text-to-stickman video generator (MVP 0.2.0). Python 3.11+, Pillow, Pydantic, FFmpeg and FFprobe. No paid AI API required.

## Install

Install FFmpeg and FFprobe with your operating system package manager. From the repository root:

```bash
python -m pip install -e '.[test]'
stickman doctor
```

## Quick start

```bash
stickman generate --prompt "A stickman learns how to save money" --duration 15 --style whiteboard --resolution 720p --aspect-ratio 16:9 --output demo.mp4
stickman validate demo.mp4
```

## Optional narration, captions, music and scene environments

Install the free `espeak` or `espeak-ng` command for local speech. Narration uses a basic synthetic voice and is clipped to each scene duration. For licensed background music, supply your own file path. The program does not download or license music for you.

```bash
stickman generate --prompt "A stickman explains saving money" --duration 15 --narration --burn-captions --output narrated.mp4
stickman generate --prompt "A person walks" --duration 15 --music /path/to/your/licensed-music.mp3 --output music.mp4
```

Edit a planned JSON scene's `background` to one of `classroom`, `school`, `office`, `home`, `park`, `city`, `street`, or `shop` for simple reusable drawn scenery. `stickman storyboard project.json --output storyboard.png` creates a contact sheet of all scenes.

## Editable workflow

```bash
stickman plan --prompt "Two characters explain compound interest" --duration 90 --output project.json
stickman storyboard project.json --output preview.png
stickman render project.json --output lesson.mp4
stickman resume project.json --output lesson.mp4
```

JSON plans contain validated characters, scenes, motions and duration. Edit `project.json` to customize motion, scene text and colors. Supported styles: `stickman styles`. Supported motions: `stickman characters`. Outputs include an MP4, an SRT subtitle file, and cached scene MP4s. Rendering streams raw frames to FFmpeg; completed scene files can be reused with `resume`. Scene cache keys include the complete project plan, so changing any plan field invalidates all cached scenes.

## Agent setup

Codex project skill: `.agents/skills/stickman-studio/SKILL.md`; invoke with `$stickman-studio`. Claude Code project skill: `.claude/skills/stickman-studio/SKILL.md`; invoke with `/stickman-studio`. Start each agent from repository root. See docs/INSTALL_CODEX.md and docs/INSTALL_CLAUDE.md.

## Limitations

This is a deterministic MVP, not a generative video model. Prompt parsing uses sentence segmentation and cycling motion presets, not semantic understanding or automatic screenplay generation. Character figures have articulated but simple geometry. Video is silent by default; local eSpeak narration, user-provided music mixing and burned captions are opt-in. No neural TTS, WebM/GIF export, camera animation, inverse kinematics, arbitrary location generation or advanced motion retargeting yet. 30-minute plans are supported; rendering 30 minutes is resource intensive and not verified. Scene cache is not safe for simultaneous writers to the same output. Fonts may differ across systems. Long captions are limited to three lines. Quality presets are not implemented.

## Contributions and licensing

MIT licensed. See CONTRIBUTING.md. Report reproducible defects with command, Python version, FFmpeg version and minimal prompt.

## Extended production capabilities (v0.3)

Dialogue lines can be written as `Alex: Hello.` and `Sam: Welcome.`. The offline planner assigns persistent character identities, speaker-specific local TTS pitch, contextual environments, simple chart props, and camera composition variants. Sidecar SRT and WebVTT files are generated. See [production guide](docs/PRODUCTION.md) for exact capabilities and limitations. Codex or Claude Code can improve the editable JSON screenplay using their own reasoning; no model API is bundled into the Python engine.
