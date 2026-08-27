---
name: codex-dream-skin-studio
description: Create, validate, save, and safely apply a Codex Dream Skin from an approved image and visual brief on Windows or macOS.
metadata:
  short-description: Build and apply Codex visual skins safely
---

# Codex Dream Skin Studio

Use this skill when a user wants to turn an image and a visual direction into a reusable Codex UI skin. The stable deliverable is a validated theme package: `theme.json`, one opaque `2560x1440` background image, and the platform renderer CSS.

## Workflow

1. Inspect the reference image and translate it into concrete layout, palette, material, light, and focal-area decisions. Keep the left side calm for native Codex content and reserve the right side for the visual subject.
2. Confirm the visual direction before generating or changing artwork. Do not copy readable brand text, watermarks, account names, or reference-image UI.
3. Validate every background with `scripts/validate-wallpaper.mjs`. Reject anything that is not PNG, JPEG, or WebP, exactly `2560x1440`, or larger than 16 MB.
4. Keep theme metadata honest. Use only supported fields; the renderer contract is `theme.json + background image + platform CSS`. Do not claim unsupported stickers or arbitrary JSON fields render.
5. Stage and save the theme before applying it. On Windows use the installed engine and `stage-theme-windows.ps1`; on macOS use the matching platform script. Saving a theme is separate from live injection.
6. Apply only through the installed Dream Skin engine. If the session has a verified CDP endpoint, use the hot path. If it does not, permit at most one explicit controlled restart. Never loop restarts or modify the Codex app bundle, `app.asar`, signatures, provider settings, or API configuration.
7. Verify the result after applying: engine version, CDP endpoint, injector status, theme identity, CSS presence, document visibility, viewport, and structure. If live injection fails, preserve the saved theme and report the exact state instead of retrying.

## Visual translation

For a clear seaside stationery / cel-animation direction:

- Use `#B9DDE5` mist blue, `#7CCFE0` water blue, `#3F8FCE` sea blue, `#FFF8E8` cream, `#F3D96B` lemon, `#F3B477` peach, and `#425B68` text.
- Use light rounded sans-serif fonts with readable Chinese fallbacks. Keep regular and medium weights; do not use heavy black text.
- Use flat paper surfaces, crisp two-level outlines, restrained hard highlights, and small offset shadows for cel-style cards.
- Use dashed postmark-style borders for major paper surfaces. Keep small controls solid so their affordance remains clear.
- Keep the composer readable: a unified mist-blue paper surface, sea-blue outer rule, transparent inner textarea, and a pale lemon focus halo.
- Keep orange for semantic actions or small accents, not as the dominant frame color.

## Windows safeguards

- Require the official Store-installed `OpenAI.Codex` package and a current upstream Dream Skin engine.
- After an engine update, restage the custom theme because installation may restore the default preset into `active-theme`.
- If `state.json` points to an injector PID that no longer matches the engine process, stop only that explicitly identified stale injector before rebuilding the session state.
- A valid `active-theme` proves the theme is saved; it does not prove live injection. Report both states separately.

## Persistence

Standard Codex launch does not guarantee external injection persistence. Provide the platform reopen launcher when requested and explain that the launcher reads the latest saved or selected theme. Do not install unsupported background agents or claim that the standard Codex icon restores the skin automatically.
