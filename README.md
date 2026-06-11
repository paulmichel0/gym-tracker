# Gym Tracker

A single-file web app for tracking workouts and bodyweight. There's no backend
and no shared database — your data is saved privately to **your own Google
Drive** (in the hidden app-data folder), so it stays on your account and nobody
else can see it.

## Want your own copy?

You can run your own independent instance. Because the app talks to Google
Drive, you need your own Google OAuth client ID (free). It takes about 10
minutes. Follow the steps below.

### 1. Fork this repository

Click **Fork** at the top of the GitHub page to create your own copy under your
account.

### 2. Create a Google OAuth client ID

1. Go to the [Google Cloud Console](https://console.cloud.google.com/) and sign
   in.
2. Create a new project (top bar → project dropdown → **New Project**). Name it
   anything, e.g. `gym-tracker`.
3. Enable the Drive API: search **"Google Drive API"** in the top search bar,
   open it, and click **Enable**.
4. Configure the consent screen: go to **APIs & Services → OAuth consent
   screen**.
   - User type: **External**.
   - Fill in the required fields (app name, your email). You can skip optional
     fields.
   - On the **Scopes** step you don't need to add anything — the app only uses
     the non-sensitive `drive.appdata` scope.
   - Under **Publishing status**, click **Publish app** (move it to
     *Production*). Because the app only uses the app-data scope, this does
     **not** require Google's verification process. If you'd rather leave it in
     *Testing*, you must add every user's Google email under **Test users**
     (max 100).
5. Create the credentials: go to **APIs & Services → Credentials → Create
   Credentials → OAuth client ID**.
   - Application type: **Web application**.
   - Under **Authorized JavaScript origins**, add the URL where you'll host the
     app. For GitHub Pages (see step 4 below) this is:
     `https://YOUR-USERNAME.github.io`
     (just the scheme + host — **no path**, no trailing slash).
   - Click **Create** and copy the **Client ID** it gives you (it ends in
     `.apps.googleusercontent.com`).

### 3. Paste your client ID into the app

In your fork, open `index.html` and find this line near the top of the script
(currently around line 245):

```js
const CLIENT_ID = '...apps.googleusercontent.com';
```

Replace the value with **your own** client ID from step 2.

### 4. Host it

The simplest free option is **GitHub Pages**:

1. In your fork, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to *Deploy from a branch*,
   choose the `main` branch and the `/ (root)` folder, then **Save**.
3. After a minute your app will be live at
   `https://YOUR-USERNAME.github.io/gym-tracker/`.

Make sure the origin you entered in step 2 (`https://YOUR-USERNAME.github.io`)
matches your actual GitHub Pages URL, otherwise sign-in will fail with an
origin/redirect error.

You can also host the file on any other static host (Netlify, Cloudflare Pages,
your own server) — just add that site's origin to the Authorized JavaScript
origins list in step 2.

### 5. Use it

Open your hosted URL, sign in with your Google account, and start tracking.
Your workouts and weight log are saved to your own Drive automatically. Sign in
with the same Google account on any device to see the same data.

## Notes

- Opening `index.html` directly from your computer (a `file://` URL) **won't
  work** for Google sign-in — Google requires a real `https://` origin that's on
  the authorized origins list. Host it (step 4) instead.
- The only thing stored locally on the device is an in-progress workout, so you
  don't lose a session if you reload or the app is closed mid-workout.
