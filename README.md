# Cat's Cradle — VHS String Interface

## Technologies

### MediaPipe Hands
Tracks both hands simultaneously in real time using a pre-trained model running entirely in the browser. Provides 21 landmarks per hand (joints and fingertips) as normalised x/y/z coordinates, which are used to position the skeleton overlay and drive the cat's cradle string endpoints.

### Canvas API (2D Context)
All visual output is drawn using the native browser Canvas 2D API across four layered canvases. The hand skeleton, glow dots, and elastic strings are drawn on the main overlay canvas each frame using paths, arcs, gradients, and shadow blur for the neon glow effect.

### RGB Split Canvases (Chromatic Aberration)
Three separate canvases are stacked with CSS `mix-blend-mode: screen`. Each canvas receives the same video frame filtered and tinted to isolate the red, green, or blue channel, then offset horizontally by a few pixels. Together they reconstruct a colour image with the fringe separation characteristic of a degraded VHS tape read.

### getUserMedia (Web Camera API)
Requests access to the device camera and streams the live feed into a hidden video element. The video element is used both as the input source for MediaPipe and as the image drawn into the off-screen buffer for the VHS rendering pipeline.

### CSS Animations and mix-blend-mode
The tape warp glitch (a periodic CSS `skewX` and `translateX` animation on the root element) and the REC dot blink are handled entirely in CSS. The three RGB canvases use `mix-blend-mode: screen` so they composite additively, which is what produces the white overlap where channels align.

### JavaScript (Vanilla ES2020)
All application logic — the render loop, VHS effect state machine, glitch line spawner, tracking jitter decay, hand landmark mapping, string physics, and tape counter — is written in plain JavaScript with no framework. `requestAnimationFrame` drives the render loop for smooth real-time performance.
