# Prompt Battle (AI Judge)

A single-file game for the AI club recruitment drive. Players see a target image, get 60 seconds and 15 words to write a prompt that recreates it, and Gemini scores the similarity from 0 to 100 and explains its score. Solo mode has a leaderboard. 1v1 mode plays both turns on one laptop.

- Image generation: [Pollinations.ai](https://pollinations.ai). With a key from <https://enter.pollinations.ai> it uses the good `zimage` model, which costs a little pollen per image. Without a key it falls back to the free keyless endpoint, which only has a weak model.
- AI judge: Gemini API free tier. You need a key from <https://aistudio.google.com/apikey>.
- Storage: browser `localStorage` for settings, the leaderboard and uploaded targets.

## Run it

**In VS Code:** open this folder. When VS Code suggests the recommended **Live Server** extension, install it. Then right-click `index.html` and choose **Open with Live Server**, or click **Go Live** in the status bar. The page reloads every time you save.

**From a terminal:**

```bash
python3 -m http.server 8000      # or: npx serve
```

Open <http://localhost:8000>, go to **Settings**, paste the Gemini key, then click **Save** and **Test connections**.

Double-clicking `index.html` usually works too. A local server is safer for image loading.

## Shared keys and target (no setup on each laptop)

Settings typed in the game live in that browser only. A different browser, laptop or site address (each Vercel preview deploy gets a new one) starts empty. So the shared setup lives in files:

- **`config.js`** holds the Gemini keys and the Pollinations key for everyone. Copy `config.example.js` to `config.js` and fill it in. List several Gemini keys from different Google accounts or projects: when one hits its daily limit, the judge moves on to the next. `config.js` is git-ignored so the keys never reach GitHub. Anyone who can open the game can read these keys, so share the link only with people you trust and delete the keys after the event.
- **`builtin-targets.js`** embeds the target image (`target-surfer-sunset.jpg`), so it's in every game. To change it, regenerate the file from a new image.

## Settings

| Setting | Notes |
| --- | --- |
| Gemini API keys | Optional when `config.js` has keys. Keys typed here are tried first. |
| Gemini model | Default `gemini-3.5-flash-lite`. If Google renames or retires it, put the current free Flash model name here. |
| Backup Gemini models | Tried in order when the main model says "high demand" or hits a rate limit. Default `gemini-flash-latest, gemini-flash-lite-latest`. |
| Pollinations key | Optional, but strongly recommended for good images. Stored only in this browser. |
| Seconds / max words | Default 60 / 15. |
| Sign-up link | Shown as a QR code on every results screen. |
| Image URL template | Swap in another generator, or change `model=` (e.g. `microsoft/mai-image-2.6-flash` for more photorealism; see the list at <https://gen.pollinations.ai/image/models>). It uses the `{prompt}`, `{seed}` and `{key}` placeholders. |
| Target prompts | Optional, one per line. The built-in image is always a target. Targets are generated from these secret prompts, so they're always reachable. The prompt is revealed after each round. |
| Upload targets | Use your own images in addition to the prompts. |

## If something breaks mid-event

- **The judge or generator fails:** the results card shows **Retry AI judge** and **Set score manually**, so the queue keeps moving.
- **Leaderboard:** use **Leaderboard → Export CSV** at the end of the day to keep the results. **Reset** clears it after asking for confirmation.

## Before October 7

1. Run 20–30 rounds back to back on the event Wi-Fi to check the free-tier rate limits.
2. Remove any target prompts that look bad or are too hard.
3. Bring a backup: a phone hotspot, and the no-code version (web image generator + Gemini chat + Google Sheet).
