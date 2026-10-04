# Forge

Make marketing videos out of your real [kreativ-ui](https://github.com/kreativ-dev) components, then post them.

Forge is a desktop studio built on [Remotion](https://www.remotion.dev). Rather than recreating your UI in a video editor, it renders live Kui components inside video compositions. Your demos, tutorials, and shorts always match your design system, because they're built from it.

> **Where things stand:** Forge is still in planning. The spec is mostly locked, but nothing is built yet. This README describes what v1 is meant to be.

## Why it exists

- **Real components, not screenshots.** Props, themes, and interaction states (hover, focus, pressed) are controlled from the timeline.
- **Local-first.** A project is just a folder. Zip it, share it, put it in git. Rendering happens on your machine.
- **Frame-native.** Everything runs on the project's frame clock. Seconds only show up at the edges of the UI.
- **For anyone using Kui,** not just its authors. Forge manages Kui versions for you.

## What's planned for v1

### The studio
- Layers tree, canvas, Properties, and Timeline panels, all dockable. One project per window.
- A Figma-style toolbar (Frame, Layout, Typography, Media, Shape, Forms, Overlays, Cursor, Import and more). It collapses to icons, then to a "more" menu, as the window narrows.
- **Properties** has two always-there groups, Transform (x/y offset, scale, rotation) and Visibility (`hidden` and `opacity`). Each component adds its own groups, like Typography, color, and size, based on its manifest.
- **Escape positioning** lets an element sit relative to any ancestor, or an ancestor's sibling, without moving it in the tree. Layout says where it lives; Escape says where it appears. Bounds can be a bounding-box multiple, fixed margins, or the canvas safe zone, and going past them raises a warning instead of clamping.
- **Pages** are self-contained scenes with their own local clock. Build the intro and outro animation once inside a page, then drop it on the Pages track. Hard cuts only in v1.

### Motion & FX
- **31 presets** across five categories:
  - Entrance: Fade In, Slide In, Pop, Rotate In, Blur In, Mask Reveal, Bounce In
  - Exit: Fade Out, Slide Out, Scale Out, Rotate Out, Blur Out, Mask Hide
  - Emphasis: Pulse, Shake, Wobble, Float, Spin, Flash, Highlight
  - Transition: Crossfade, Swap, Morph, Push
  - Text: Typewriter, Fade Up, Slide In, Stagger Reveal, Scramble, Highlight Sweep, Count Up
- Presets are editable when you apply them (duration, easing, delay, plus per-preset options) and can be stacked. They bake down to ordinary keyframes.
- **Text Split** breaks text into words or characters so each piece can animate on its own.
- **Effects** are separate from presets: Particles, Glow, Shake, click Ripple, and Echo.
- **Cursor** is a fake mouse that can Move To, Click, and box-select, so you can stage a realistic walkthrough.
- **Timeline tools:** keyframes, bezier curve editor, snap, markers, stagger, range, copy/paste keyframes, time stretch, reverse, and linked properties.

### Media
- **Device Frame:** a parametric phone, tablet, or desktop mockup wrapping an image or video. You build the device yourself (size, corner radius, notch, status bar, nav bar, buttons) instead of picking from a catalog of real devices that would go stale.
- **Assets:** images, video, audio, fonts, code snippets, and SVG icons. Forge seeds every project with its own icon set.
- **Playground import:** bring components you've already composed in k-playground straight into Forge.

### Output
- Local render with Remotion, headless Chromium, and FFmpeg.
- 9:16 (1080×1920) by default, with per-project export overrides.
- Four starter templates: Theme Swap Reveal, Code + Result Split, Code + Use Cases, and Philosophy Card.

## How it works

Forge is an Electron app with three parts:

| Part | Job |
|---|---|
| **Main** | Owns project files, the render queue, the Kui version cache, and credentials |
| **Renderer** | The Studio UI: Layers, Canvas, Properties, Timeline |
| **Render Worker** | Remotion, headless Chromium, and FFmpeg |

For a component to work in video, its output has to depend only on the current frame. That means no internal timers, CSS transitions turned off during render, and hover, focus, and open states controlled from outside.

## Project format

```
MyProject/
├── project.json    elements, tracks, keyframes, markers, compositions, theme override
├── media/          images, video, audio
├── icons/          SVG icons
├── fonts/
├── snippets/       custom code snippets
└── thumbnails/
```

- `project.json` is validated strictly. Unknown fields are errors, never quietly dropped.
- Each project pins its own `runtime.forgeVersion` and `runtime.kuiVersion`, so old projects keep rendering the way they used to.
- Components are referenced by stable ids. No Kui source gets copied into your project.
- Themed values use typed references: `{ "$semantic": "bezel" }`, `{ "$token": "colors.gray.500" }`, `{ "$typography": "heading" }`, `{ "$size": "md" }`. A plain string is always just a string.
- Each project can extend the Kui theme with its own tokens, typography presets, and sizes, without touching Studio's own look.

## Kui versions

Forge ships with two or three stable Kui versions bundled, so you can start a project offline. Other versions install on demand into a local cache and sit side by side, so opening someone else's project just works. Pick a version per project in Settings.

## Roadmap

| Version | What's in it |
|---|---|
| **v1** | Everything above |
| **v1.1** | Pen Draw, and Zoom To (camera zoom and pan) |
| **v2** | Team projects and online sync, live content inside Device Frames, reusable page instances |

## Development

Not available yet. Build and run instructions will go here once the first milestone lands.

## License

Licensing is split by component:

- **Kreativ Forge (the app):** Proprietary. All rights reserved.
- **Forge SDK and integration interfaces:** may be open-sourced later.
- **Templates and assets:** licensed separately under commercial terms where appropriate.

Kreativ UI is licensed on its own terms and isn't covered by the above.
