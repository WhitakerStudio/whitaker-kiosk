# iPhone WebM looping diagnostic

Upload this folder as /looptest/ on kiosk.whitakerstudio.com. Do not replace
the production QR or NFC pages with this file. It reuses the existing
/qr/assets/data/sculptures.json and /qr/assets/images/ files on that hostname.
No SQL changes, copied media, or JSON regeneration are required.

First open:
https://kiosk.whitakerstudio.com/looptest/?d=Windflower&mode=native

Keep the video visible for one minute. Note whether it stops. Then select
"B — JavaScript restart after each play" and press "Start a fresh test".
Keep that video visible for one minute as well. Each mode starts a fresh
page/player using the same video, muted inline playback and initial
programmatic play attempt. If autoplay is rejected, the page says so and
offers Pause / resume; report whether you needed that button.

The native test uses loop=true. The manual test uses loop=false and restarts
on the ended event. Neither includes the production scroll observer,
watchdog restart, or GIF fallback. Manual repeats may have a small seam.
Both respect backgrounding and the Pause / resume button. Media events and
status are only displayed locally on this page, not uploaded. The page
adds no identifiers, cookies, local/session storage or scan alerts. Normal
hosting request logs are outside this page's control.

Please report results for A and B separately, with a screenshot of the
readings and expanded Recent media events if either freezes. Browser
readiness/network state numbers are diagnostics, not success indicators.
Repeat counts in native mode are observed timeline wraps and approximate.

Interpretation:
- Only A stops: native looping is a candidate; test manual restart in the
  production controller next (not proof it is fixed there).
- Both stop: investigate the shared browser/encoding/serving path, not just
  the production controller. A different encoding of one file is a useful
  next controlled comparison.
- Neither stops: investigate production lifecycle/controller behavior.

Validation: local mocked tests cover twenty manual repeats, native mode,
pause/resume, background/return, missing media and absence of scan logging.
Actual iPhone decoding and smoothness have not been verified here.
