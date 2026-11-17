# Run, Little Toaster: silly 2D music video (three.js, exported for YouTube)

## Context
The user wants a silly, animated **2D** music video for their indie-pop song "Run, Little Toaster" (lyrics pasted in chat). It should be built with three.js, and the final product is a **video file to upload to YouTube**. The MP3 isn't in `C:\BrianFolder\AI Generated` yet, and the folder has no existing code. The machine has Node and Chrome but no ffmpeg, so the export has to happen inside the browser.

## Output
A new folder, `C:\BrianFolder\AI Generated\Toaster Video\`, containing:
- `index.html`: player, preview and exporter in one page. three.js 0.160.0 and `mp4-muxer` 5.1.3 load as ES modules from cdn.jsdelivr.net, with no build step.
- `timeline.js`: section start times and lyric lines with timestamps, all editable in one place.
- `toaster.mp3`: the user drops the song in here. A file picker also accepts any MP3.
- The result: a **1920×1080, 30fps MP4 (H.264 + AAC)** downloaded from the page, ready for YouTube.

## 2D look: flat cartoon, built in code with no image files
- three.js with an **OrthographicCamera**. Every character is a flat shape (`ShapeGeometry`, MeshBasicMaterial) with thick dark outlines (a slightly larger black copy behind each shape), in candy colors, stacked in layers for parallax.
- **Toaster hero**: a chrome-silver rounded rectangle with a white shine streak, two slot lines, big googly eyes whose pupils jiggle on springs, a determined eyebrow, stick legs in red sneakers, and its cord tied around its head like a sweatband. Toast pops out of its head on big beats.
- **Squash and stretch** on every hop, rubber-hose leg run cycle, speed lines, dust puffs, and sweat drops that fly off.
- **Silly cast**: bagels with judgy eyes, toast slices in tracksuits, a jam jar referee with a whistle, and a flour sack that faints.
- **Scenes**, one per story beat, built from side-scrolling parallax layers:
  1. **V1, kitchen before dawn**: a dark blue counter. The toaster's eyes snap open, it ties on the cord, and a sunrise fills the window.
  2. **Pre-chorus**: it runs along the counter edge, takes a huge cartoon leap to the checkered floor and boings, while step counters ("1… 2… 3…") pop up.
  3. **Chorus**: a side-scrolling sprint past the fridge. Lyrics bounce in big chunky type, bread chases or cheers, and toast pops on the beat.
  4. **V2, store backroom**: crates, flour and jam shelves, bagels that slowly turn to stare, and a swinging door that slaps the camera.
  5. **Pre-chorus 2**: "lifts one shiny wheel" turns into rocket-boost flames from its sneakers.
  6. **Bridge, stadium**: a crowd of bread, a siren light, slow motion, and a dramatic close-up on the eyes for "Look at me".
  7. **Final chorus**: everything doubles in speed. A pan-flip trophy moment, it crosses the finish tape, and confetti, a medal and a fireworks burst follow.
  8. **Outro**: a sunset, the toaster on the podium with a medal and a single slice of toast saluting, then title text.

## Sync to the song
- Everything is a pure function of time: `render(t)`. The same code runs the live preview and the frame-exact export, so the export can never drift from the preview.
- **Beat reactions**: the MP3 is decoded once with `OfflineAudioContext`. The page precomputes a kick-band energy envelope and estimates BPM, and `beat(t)` drives bounces, flashes and toast pops.
- **Lyrics**: chunky captions are drawn **inside the canvas** (a CanvasTexture plane), so they end up in the exported video. The hook gets a bigger pop and an echo trail.
- **Tap-sync helper**: press `T`, play the song, and tap space at each section and lyric line. The page prints a new `timeline.js` to paste in. Placeholder timings (about 2:45) ship until the MP3 arrives.

## Export to MP4, in the browser
- Render each frame `i` at `t = i/30`, then use WebCodecs: a `VideoEncoder` (avc1, 1080p, ~12 Mbps) and an `AudioEncoder` (AAC 192k) feeding the decoded song. `mp4-muxer` combines them into one file, which then downloads.
- Rendering runs offline, so no frames are dropped no matter how fast the PC is. A progress bar is shown.
- Fallback: if Chrome lacks the AAC encoder, the page does a real-time `MediaRecorder` capture to WebM (canvas plus audio), which YouTube also accepts.

## Build order
1. Scaffold: renderer, ortho camera, layers, the `render(t)` loop, audio loading and analysis, and `timeline.js`.
2. Toaster rig: shapes, outline helper, run cycle, squash/stretch and the googly-eye spring.
3. Scenes 1–8 and the supporting cast.
4. Caption plane, beat reactions and particles (dust, sweat, confetti, toast).
5. MP4 export, then tap-sync mode.
6. When the MP3 lands, tap-sync and tune the timings, then export the final video.

## Verification
- Serve the folder with `npx http-server` and open it in Chrome via the claude-in-chrome tools. Check the console for errors and screenshot each scene by scrubbing to it.
- Export a short 10-second test clip and confirm it plays with audio in sync (open it in Chrome/VLC).
- After the song arrives: do a full preview pass, a full export, and spot-check the MP4 at several timestamps against the lyrics.
