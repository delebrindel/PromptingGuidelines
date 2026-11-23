---
name: screenshot-capture
description: Capture and verify portfolio screenshots with a fixed viewport, demo data, and a byte comparison against the committed images before replacing them. Use when retaking project screenshots or updating images in a portfolio repo.
---

# Screenshot capture

Screenshots are portfolio assets. Capture from the running app with demo data. Replace a committed image only when the new capture differs for a reason you can name.

## Setup

1. Point the app at demo data, never personal data. For Spento that is the `spento_demo` database. Change `db.name` in `config.php` for the capture and restore it afterward. `config.php` is gitignored; do not print it.
2. Log in with the demo account. Keep credentials out of files, logs, and commits.
3. Confirm the state you want is on screen. For example, category names should be the English ones after migration `011`.

## Capture

- Viewport: 390×844 at device scale factor 2 (@2x). Use the same size for every view in a project.
- One PNG per view, named for the view. Spento views: `home`, `categories`, `budgets`, `activity`, `reports`, `add-expense`, `add-income`.
- Wait for the page to finish: no spinners, charts fully drawn.
- Check the viewport and device scale factor before capturing. Use the browser tool available in the session (Playwright or Chrome DevTools).
- Save raw captures outside the repo, for example `D:/<project>-shots/live/`.

## Compare before replacing

1. For each view, compare the new PNG with the committed file using `cmp` or `sha256sum`.
2. Identical: nothing to change. Different: open both and confirm the difference is intended (text, data, layout) before copying.
3. Copy only the views that changed into the repo's asset folder. For Spento: `public/assets/projects/spento/`.
4. Check the page that displays the image after copying.

## Rules

- Do not commit. Leave changed images in the working tree and list them.
- Do not capture from a live production account or personal data.
- Do not retouch images by hand. Retake the capture if it is wrong.
- Restore `config.php` to its normal database and confirm it.

## Checklist

- [ ] Demo data, state confirmed
- [ ] Viewport 390×844 @2x verified
- [ ] Each view compared byte-for-byte with the committed file
- [ ] Only intended images copied
- [ ] `config.php` restored
