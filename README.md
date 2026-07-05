# Jarhead Outpost — invite / install page

One static page (`index.html`, no build step, no dependencies) that a Marine lands on when they
tap an invite link. It detects the phone and shows the right button:

- **iPhone** → TestFlight
- **Android** → the APK
- **Desktop / unknown** → both (so they can send it to their phone)

This is the durable target for the app's `INVITE_URL` — one stable link that never rotates, even
as individual builds change.

## 1. Fill in the two links

Edit the top of the `<script>` block in `index.html`:

```js
var TESTFLIGHT_URL   = "https://testflight.apple.com/join/XXXX";     // TestFlight public link
var ANDROID_APK_URL  = "https://expo.dev/accounts/.../builds/XXXX";  // EAS build install URL
```

- **TestFlight link:** App Store Connect → your app → TestFlight → **Public Link** (create one
  for an external group).
- **Android link:** expo.dev → project → **Builds** → the latest `preview` build's install URL.
  (Later, for a link that never changes, host a `app.apk` here and point at `/app.apk`.)

## 2. Deploy (pick one — both free)

**GitHub Pages**
```bash
cd jarhead-outpost-site
git init && git add -A && git commit -m "Invite page"
gh repo create jarhead-outpost-site --public --source=. --push
# GitHub → repo → Settings → Pages → Deploy from branch: main / root
```
URL: `https://<you>.github.io/jarhead-outpost-site/`

**Vercel** (nicer URL, instant)
```bash
cd jarhead-outpost-site
npx vercel        # follow prompts; it serves index.html as-is
```
Optionally attach a custom domain (e.g. `jarheadoutpost.app`) in the host's dashboard.

## 3. Point the app at it

In the app repo, set `INVITE_URL` in `src/invite.ts` to this page's URL, then rebuild. The
"Invite a Marine" button now shares one durable link that works for both platforms.

---

Keep it invite-only in spirit: this page is just the front door for Marines who already got a
link from another Marine — not a public marketing site.
