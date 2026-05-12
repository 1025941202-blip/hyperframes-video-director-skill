---
name: hyperframes-video-director
description: Use this skill whenever the user wants to turn a Chinese script into a publishable short video with HyperFrames, Image Gen/Image2 visuals, screen recordings, cloned voice narration, synced one-line captions, and final QC. Trigger this for requests like 用 HyperFrames 做视频, Codex 自动剪视频, 文案转视频, 加录屏, 用我的声音配音, 字幕对不上, 画面遮挡, 成片自查, or when revising a HyperFrames video after user feedback.
---

# HyperFrames Short Video Director

## Scope

Use this skill to create or revise vertical short videos from a script, especially Chinese explainer videos that combine:

- HyperFrames HTML animation
- Image Gen or Image2 visual assets
- screen recordings
- cloned voice narration
- synced captions
- final exported MP4 review

The goal is not just to render a video. The goal is to deliver a publishable video that has clear rhythm, readable captions, matched voice timing, and no visual overlap.

## Core Principle

Treat the script as the source of the video, but treat the final voice audio as the source of subtitle timing.

Do not lock captions to the first draft timeline. Once narration changes, regenerate or retime captions from the final audio.

## Required Inputs

Collect or infer these before production:

- Final script text.
- Target format, usually 1080x1920 vertical for short video.
- Whether the user wants generic TTS or a specific cloned voice.
- Optional voice clone project path. The original reference path is `/Users/mac/Desktop/Obsidian/AI声音克隆`; teammates may choose this path if it exists on their machine, or provide their own voice clone project path.
- Any screen recordings or product footage.
- Whether Image Gen/Image2 may be used for supplemental visuals.
- Caption style constraints, especially punctuation, line count, size, and vertical position.
- Output location and whether the result must also be editable later.

If the user gives a revised script after a draft video exists, update voiceover and captions together.

## Default Video Workflow

### 1. Lock the Script

Before generating voice or captions, normalize the script into short spoken beats.

For Chinese short video, keep beats short:

- One idea per beat.
- Avoid long subtitle lines.
- Preserve user wording unless the user asks for rewriting.
- Do not add punctuation to captions if the user requested clean captions.

If the user provides a text file, read the file and treat it as the latest source of truth.

### 2. Build a Shot Plan

Split the script into video scenes:

- Hook: why the viewer should care.
- Method: what the user does.
- Operation: where screen recording belongs.
- System explanation: what Codex or HyperFrames is doing.
- Value: why the workflow is better.
- Ending: memorable close.

For each scene, decide the visual role:

- Title card for a big concept.
- Card/grid for process or comparison.
- Screen recording for concrete operation steps.
- Image Gen/Image2 for abstract or hard-to-film ideas.
- Simple motion graphics for workflow, rhythm, and transitions.

### 3. Use Screen Recordings Selectively

Screen recordings are not background decoration.

Use them only when the voiceover is talking about operation, interface, real workflow, or "录屏".

If a screen recording overlaps or blocks the original animated design:

- Remove or hide the recording only for the conflicting segment.
- Do not delete all screen recordings unless the user explicitly asks.
- Prefer trimming, resizing, cropping, moving, or shortening the recording.
- Keep it away from main titles, cards, and caption boxes.

When reviewing, explicitly check the operation segments where recordings appear.

### 4. Generate Visual Assets Only Where They Help

Use Image Gen/Image2 when the script needs visual enrichment and no real footage exists.

Good uses:

- "AI 图片" or "真实感画面"
- abstract workflows
- before/after concepts
- cinematic visual metaphors
- product or tool scenes that need atmosphere

Bad uses:

- replacing a real screen recording when the point is an operation demo
- generating unrelated decorative images
- adding images that fight with titles and captions

Images must match the exact script beat they support.

### 5. Generate or Import Voiceover

If the user provides a voice clone project or says "用我的声音", use that instead of generic HyperFrames TTS.

Voice clone path selection:

- First ask or infer which voice source the user wants.
- If the user wants the known local clone project, use `/Users/mac/Desktop/Obsidian/AI声音克隆` as the reference path.
- If that path does not exist on the current machine, ask the user for their own clone project path.
- Do not publish or copy private voice samples, checkpoints, or generated audio into a public Skill repository.
- The Skill may document the path, but the actual model files remain local to the user's machine.

Recommended order:

1. Generate final narration from the final script.
2. Verify duration and audio exists.
3. Use this audio as the timing anchor.
4. Only then build or retime captions.

Do not judge sync by the old video timeline after replacing voice.

### 6. Caption Rules

Captions must be designed for mobile viewing.

Defaults:

- Single line.
- Large enough to read at phone size.
- No punctuation if the user requested no commas or periods.
- No stray one-character wrap onto a second line.
- Position above the bottom UI safe area.
- Do not overlap cards, phone mockups, screen recordings, product UI, or big title text.
- Keep enough vertical gap between captions and the nearest foreground element.

Implementation hints:

- Use `white-space: nowrap`.
- Use fixed left/right safe margins.
- Use `overflow: hidden` only as a guard, not as a way to hide bad text.
- Use smaller classes for long lines, but avoid shrinking too far.
- Prefer splitting one long spoken sentence into two timed caption beats.

### 7. Sync Captions to Final Audio

Use the most reliable available source:

- If HyperFrames transcription works, use transcript timing.
- If not, use silence detection and manual beat mapping.
- If there is a text file with one line per spoken beat, map those lines to audio pauses.
- For cloned voices, expect timing to differ from generic TTS.

Check sync by sampling:

- caption starts
- long pauses
- transition moments
- ending lines

If the user says "字幕跟声音对不上", do not only move all captions by one offset. Rebuild the caption timing from the final audio.

## HyperFrames Build Rules

Follow the HyperFrames skill and CLI workflow:

1. Create or edit the HTML composition.
2. Run `npx hyperframes lint`.
3. Run `npx hyperframes inspect`.
4. Render a review MP4.
5. Extract keyframes/contact sheets from the rendered MP4.
6. Inspect the actual rendered frames.
7. Fix issues and render again.

Important composition rules:

- HTML is the source of truth.
- `data-duration` defines clip duration.
- Use `data-track-index` to avoid timed clip conflicts.
- Use CSS `z-index` for visual layering.
- Video elements must be muted and `playsinline`.
- Audio belongs in a separate audio element.
- Animate from the final layout, not from guessed positions.
- Avoid slow entrances that leave captions visible before the scene content has arrived.

## Mandatory Self-Review

Do this before showing the user the final video.

### Automated Checks

Run:

- `npx hyperframes lint`
- `npx hyperframes inspect --samples 24`
- `npx hyperframes inspect --at <important timestamps>`

Important timestamps include:

- first title fully visible
- each screen recording segment
- each long caption
- scene transitions
- the ending

Warnings about file size or dense tracks can be acceptable for a small one-off video, but layout issues must be fixed.

### Rendered-Video Checks

Do not rely only on the HTML or tool output. Inspect the exported MP4.

Check:

- first frame readability
- title readability
- caption position
- caption size
- caption single-line behavior
- caption and voice sync
- screen recording placement
- screen recording does not cover original animation
- no title/card/image overlaps with subtitles
- ending is not weak or duplicated
- video duration matches narration
- output file opens locally

Create a contact sheet or keyframe audit when possible. Review the actual pixels.

### Common Failure Fixes

If subtitles are too low:

- Move the caption container upward.
- Check it against phone playback controls or platform UI.

If subtitles overlap visual elements:

- Move the visual element.
- Shorten or split the subtitle.
- Move the caption vertical position.
- Do not assume `inspect` catches every aesthetic overlap.

If screen recording blocks animation:

- Hide only the conflicting recording segment.
- Keep useful operation recording elsewhere.

If captions are out of sync:

- Use final cloned narration timing.
- Split captions by detected pauses.
- Re-render and sample caption-start frames.

If the user says only one section changed:

- Audit the whole video, not only the first section.
- Confirm later scenes also reflect the new script, voice, and caption rules.

## Revision Workflow After User Feedback

When the user reports problems, classify each one:

- Content issue: script, wording, or scene choice.
- Voice issue: wrong voice, bad cloned voice, volume, timing.
- Caption issue: wording, position, size, punctuation, sync.
- Screen recording issue: missing, too much, misplaced, covering design.
- Visual issue: overlap, weak design, wrong asset, bad transition.

Then fix all affected layers together.

Examples:

- Changing the script means regenerate or replace voice, then retime captions.
- Changing voice means retime captions, even if the script is unchanged.
- Moving screen recording means recheck both captions and original motion graphics.
- Raising captions means recheck every scene, not only the frame the user showed.

## Project Memory Rule

If the user asks to write lessons into project memory, first show the proposed memory entry in chat and ask for explicit confirmation. Do not directly persist long-term memory without confirmation when the project rules forbid it.

Useful memory entry pattern:

```md
## HyperFrames video QC

- Screen recordings only appear in operation segments.
- If a recording overlaps animation, trim or hide only that conflict segment.
- Captions stay one line, large, and above the bottom safe area.
- Captions must be retimed from the final narration audio.
- Before delivery, inspect rendered keyframes for overlap and sync.
```

## Done Criteria

The task is done only when:

- The final script has been used.
- The requested voice has been used.
- Captions match the final voice timing.
- Caption style constraints are satisfied.
- Screen recordings appear only where they help.
- Overlap checks pass in rendered frames.
- The exported MP4 exists and opens locally.
- The user receives the exact output path.

## Hand-Off Format

When finished, report:

- Final MP4 path.
- What changed.
- What checks passed.
- Any remaining limitation, such as a transcription model unavailable or a source recording too small.

Do not over-explain tool internals unless the user asks.
