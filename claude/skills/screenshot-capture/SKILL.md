---
name: screenshot-capture
description: Capture and verify project screenshots with a fixed viewport, demo data, and a byte comparison against the committed images before replacing them. Use when retaking screenshots or updating images in a repo that displays them.
---

# Screenshot capture

Screenshots committed to a repo are assets. Capture from the running app with demo data. Replace a committed image only when the new capture differs for a reason you can name.

## Setup

1. Point the app at demo data, never personal data. Change the database name in the app's config for the capture and restore it afterward. Keep config files that hold credentials out of output and commits.
2. Log in with the demo account. Keep credentials out of files, logs, and commits.
3. Confirm the state you want is on screen, such as the labels or data the screenshot should show.

## Capture

- Viewport: 390×844 at device scale factor 2 (@2x). Use the same size for every view in a project.
- One PNG per view, named for the view (for example `home`, `settings`, `detail`).
- Wait for the page to finish: no spinners, charts fully drawn.
- Check the viewport and device scale factor before capturing. Use the browser tool available in the session (Playwright or Chrome DevTools).
- Save raw captures outside the repo, for example `<scratch-root>/<project>-shots/live/`.

## Compare before replacing

1. For each view, compare the new PNG with the committed file using `cmp` or `sha256sum`.
2. Identical: nothing to change. Different: open both and confirm the difference is intended (text, data, layout) before copying.
3. Copy only the views that changed into the repo's image folder, for example `public/assets/projects/<project>/`.
4. Check the page that displays the image after copying.

## Rules

- Do not commit. Leave changed images in the working tree and list them.
- Do not capture from a live production account or personal data.
- Do not retouch images by hand. Retake the capture if it is wrong.
- Restore the app's database config to its normal database and confirm it.

## Checklist

- [ ] Demo data, state confirmed
- [ ] Viewport 390×844 @2x verified
- [ ] Each view compared byte-for-byte with the committed file
- [ ] Only intended images copied
- [ ] Database config restored
