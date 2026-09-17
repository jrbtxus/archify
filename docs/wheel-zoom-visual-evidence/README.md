# Pointer-anchored wheel zoom visual evidence

This revision adds no new images: the two captures below are reused unchanged
from the original pull-request revision (documented 2026-09-02). Automated
browser evidence and perceptual review are reported separately, and what this
environment could not verify is listed as untested rather than implied.

## Automated browser evidence

- Command and revision: `ARCHIFY_CHROME=<chrome> node --test
  test/wheel-zoom.test.mjs` in `archify/`, plus `npm test` and
  `npm run test:browser` at revision `4e25478` (Node 22.23.2, headless
  Chromium 131).
- Results: wheel-zoom suite 3/3 pass; `npm test` 1653 tests with 0 fail
  (1588 pass, 65 skipped browser suites); the shared browser gate 226/226 pass,
  0 skipped.
- Input: trusted Chrome `Input.dispatchMouseEvent` mouse-wheel events
  (`isTrusted`), never synthetic `dispatchEvent` calls.
- Asserted: pointer-anchored zoom with the X and Y anchor invariant, the reset
  identity transform, `Ctrl+wheel` pass-through, page scrolling at the 1x lower
  bound and the 3x upper bound, and that the excluded navigation overlay and
  horizontal input neither zoom nor call `preventDefault()`.
- Conditions: workflow `agent-tool-call.workflow.json`, `showcase` quality, dark
  theme, 1440x900 and 1440x700 desktop viewports.

## Reused images

![Before wheel zoom](before-wheel.png)

![After one upward wheel at the pointer](after-wheel-zoomed.png)

- Before: `1x`, full workflow diagram.
- After: one `-120` mouse-wheel event at the pointer, `1.5x`.
- The content under the pointer is preserved in the viewport; the surrounding
  diagram expands around that anchor point instead of zooming from the SVG
  center.

## Perceptual visual review

Status: **skipped** for this revision. No human review of this revision's
rendering was performed. The original revision reported its own review of these
images; that claim is not re-made here, and this revision does not change the
zoom-in rendering these images show.

## Not tested

- Physical trackpad and two-finger scrolling, `Ctrl`/pinch gestures, and OS-level
  scroll or acceleration settings. The wheel input is headless Chrome's trusted
  CDP input, not a device session, so trackpad timing, inertia and
  platform-specific gesture behavior are not claimed.
- No device, OS or broad perceptual acceptance is claimed.

The PNGs are deterministic evidence artifacts; the generated HTML is not checked
in because the public renderer tests already cover it and the full viewer runtime
would be duplicated here.
