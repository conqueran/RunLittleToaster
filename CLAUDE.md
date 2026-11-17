# Run, Little Toaster: music video project

This is a silly, animated 2D music video for the user's indie-pop song "Run, Little Toaster". It is built with three.js and will be exported as an MP4 for YouTube.

## Status (as of 2026-11-17)
- `samples.html` is a preview page with four animated sample scenes. It is published as a private artifact at https://claude.ai/artifact/8CdHm6nJ6fhPKRNePYEGyy.
- The user asked to see samples before the full build. **Next step:** get their feedback on the look, then build the full video according to `PLAN.md`.
- The song MP3 is not here yet. The user will add it as `toaster.mp3`. The samples use a placeholder 120 BPM beat until then.

## Decisions the user made
- **three.js** is the rendering library (the user asked for it).
- **2D, animated, silly.** Use a flat cartoon style with an orthographic camera, thick ink outlines and candy colors.
- **The final output is a video file for uploading to YouTube.** The plan is to export a 1080p30 MP4 in the browser using WebCodecs and mp4-muxer, because ffmpeg is not installed on the original machine.

## How samples.html is built
- It uses three.js r128 loaded from cdnjs (the UMD build). Everything is drawn in code, with no image files.
- Shape helpers: `rr` (rounded rectangle with outline), `circ`, `ring`, `frame`, `limb` (pivots at its top, with `.end` for chaining), `gradPlane`, and `label`/`makeCaption`, which draw text onto a CanvasTexture in the Luckiest Guy font.
- Character builders: `makeToaster()` (chrome body, googly eyes with lids, the cord tied on as a sweatband with a plug tail, sneakers, glove hands, toast in the slots), `poseIdle`, `poseRun(T, t, freq, amp)`, `makeBagel`, `makeFan`, `makeBuddy`, and `makeToast`.
- Each scene is `{tag, name, dur, poster, scene, hud, update(t, cam)}`. Every scene is a pure function of time `t`, so the preview and a frame-by-frame export always match. Lyrics are drawn in a separate HUD scene, so camera zoom doesn't affect them.
- The four scenes are Dawn (Verse 1), Sprint (Chorus), Bagel Stare-Down (Verse 2) and Finish Line (Final chorus).

## Lyrics
```
[Verse 1]
He wakes before dawn / In a chrome little box / Dust on the cord / But he will not stop
Two slots for a heart / And a spring in his spine / He ties up his cord / Says, "This time is mine"

[Pre-Chorus]
Past the counter edge / Past the tiled floor / He's counting every step / Then he asks for more

[Chorus]
Run, little toaster, run / Run, little toaster, run / Show the bread what you can do / I can do things too
Run, little toaster, run / Run, little toaster, run / Four crusty miles and a dream / I'll make it clean

[Verse 2]
He jogs by the crates / At the back of the store / Past the flour and the jam / Past the swinging door
The bagels all stare / At his stubborn face / He says, "Watch me now" / And he keeps his pace

[Pre-Chorus]
Every tiny jolt / Every burning doubt / He lifts one shiny wheel / And lets it out

[Chorus] (repeat)

[Bridge]
He's not just morning heat / He's more than crumbs and toast / He wants a medal too / He wants it most
So when the siren blows / And the crowd leans in / He says, "Look at me" / Then he starts again

[Final Chorus]
Run, little toaster, run / Run, little toaster, run / Show the bread what you can do / I can do things too
Run, little toaster, run / Run, little toaster, run / Cross that line, lift that pan / Yeah, I knew you can

[Outro]
Click-clack, bright and brave / Across the finish line / One small kitchen dream / At a perfect time
```

## Song style notes (for syncing visuals)
Indie-pop with a driving four-on-the-floor pulse and bouncy guitar stabs. The verses ride on tight drums, palm-muted bass and spoken-sung lines. The pre-chorus strips down to kick, claps and a rising synth fizz. The chorus opens up with stacked gang vocals and a bright chant hook. The vocals have a doubled lead, call-and-response ad-libs and delay throws on the key phrase. There are sneaker squeaks and match-strike risers, and the last chorus lifts an octave. The mix is crisp, playful and punchy.
