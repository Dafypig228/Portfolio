FORTHOSV — Portfolio
The first ever CRT-designed portfolio website. Built from scratch by Abdildenov Adil · 03.06.2026

A single-file portfolio that boots like a 1990s television set. Every pixel on screen is rendered through a WebGL CRT shader — curvature, shadow-mask phosphors, scanlines, chromatic aberration, grain, bezel vignette, and a power-on sweep. There is no framework, no build step, no dependencies. Just one index.html.

Live Demo
Open index.html in any modern browser. That's it.

For mobile and accessibility, a clean ATS-style résumé renders as a fallback for users with prefers-reduced-motion or no WebGL.

What Makes It Different
Most "retro CRT" sites slap a scanline overlay on top of normal HTML. This one doesn't.

The entire page is drawn into a <canvas> — typography, navigation, project cards, everything. There is no DOM under the glass.
The canvas is then uploaded as a texture to a custom GLSL fragment shader that simulates the physics of an aperture-grille CRT.
Two textures, two render modes. Large display text gets the full heavy-mask treatment; small body text uses a second "relief" texture with a finer phosphor cell so it stays legible without losing the CRT look.
Hit-testing happens in curved space. Click coordinates are inverse-mapped through the same barrel distortion the shader applies, so links and nav buttons remain clickable exactly where they appear on the curved screen.
Features
CRT Simulation
Barrel curvature with edge vignette and bezel
Aperture-grille shadow mask with offset slot pattern
Per-line horizontal scanlines synced to virtual resolution
Chromatic aberration that scales with distance from center
Phosphor washout on bright luma
Animated grain + brightness flicker (60 Hz hum + slow drift)
Power-on sweep ("CRT warm-up") and channel-change static between pages
Interactive Type
The FORTHOSV ASCII logo is built cell-by-cell. Each glyph picks a random scramble character on a randomized interval, so the logo is always quietly "tuning."
Hovering the mouse near the logo locks nearby cells into red accent color and re-scrambles them — a tactile, magnetic effect.
Pages
HOME — animated ASCII identity
PROJECTS — slideshow with prev/next, dot indicator, deep-linked YouTube preview
ABOUT — education + skills grid
CONTACT — clickable email / GitHub / Instagram / Telegram
Keyboard
1 2 3 4 — jump to nav tabs
Esc — return to HOME
Fallback
No WebGL or prefers-reduced-motion? You get a print-clean, recruiter-ready résumé page with proper semantic HTML.
Architecture
index.html
├── <style>      → bezel + ATS fallback résumé
├── <main.fb>    → semantic resume (SEO + a11y + no-JS fallback)
├── <canvas#gl>  → CRT screen
└── <script>
    ├── CONFIG          → every visual knob in one place
    ├── PAGES           → content as data, not markup
    ├── 2D canvas (page)   → ink layer, drawn each frame
    ├── 2D canvas (page2)  → relief layer (small-text-only)
    └── WebGL shader       → reads both textures, outputs CRT pixels
Why two textures?
To make small text more readable. I could use size based texture but it would not work on ASCII Forthosv text so i made a selectable textures.

Why draw into canvas instead of DOM?
Because the shader needs a single image to distort. CSS transform can't bend text. Putting everything through canvas means typography, layout, and the CRT effect are all driven by the same pipeline — no jank between layers, no z-fighting, no overlay seams.

Configuration
Open index.html, find the CONFIG block at the top of the script, tweak any value, refresh.

Knob	What it does
curvature	How aggressively the screen bulges
frame	Outer bezel inset (1.0 = full screen, >1.0 reveals bezel)
aberration	RGB channel split strength
grain	Per-pixel noise level
maskStrength	Shadow-mask darkening — higher = more visible phosphors
maskCell / reliefMaskCell	Phosphor cell size for headlines / body text
scanDepth / reliefScanDepth	Scanline contrast for headlines / body text
gain / reliefGain	Brightness compensation after mask multiply
asciiScrambleHz	How fast the logo rerolls (Hz)
asciiHoverRadius	Cursor magnetism radius around the logo
bootMs / transMs	Boot animation and page-change duration
The paper, ink, dim, accent, and rule color tokens drive the entire palette. Change them and the whole site re-themes.

Performance
One shader pass per frame, two texture uploads per page change (not per frame).
The canvas is only re-rendered when:
the window resizes,
the page changes,
the user clicks a project dot,
the user moves the mouse over the logo,
or the ASCII scramble timer ticks (10 Hz, only on HOME).
Idle pages render at zero cost — the shader keeps drawing the same texture.
Target: 60 fps on integrated graphics. Texture size is clamped to min(devicePixelRatio, 2).

File Structure
WebsiteV2/
├── index.html        # The entire site
├── index — копия.html  # Backup of an earlier working version
└── README.md         # You are here
That's the whole repo. No node_modules, no dist, no package.json. The site loads in one HTTP request.

Stack
Vanilla HTML / CSS / JS — no framework, no bundler
WebGL 1 — fragment shader for the CRT effect
Canvas 2D — typography and layout rendering
Zero dependencies — opens offline, deploys anywhere static
About the Author
Adil Abdildenov — Creative Developer, Astana, Kazakhstan.

Lead Programmer, FIRST Tech Challenge Team xCeption (3rd Place Think Award, FTC World Championship 2026, Houston)
Builder of DAVOS — a local-memory AI companion
Builder of B.R.E.D — a text-rendered horror engine in C++/Godot
Reach me:

Email — adil.abdilden2010@gmail.com
GitHub — @Dafypig228
Telegram — @Forthosv
Instagram — @eexactly_f
