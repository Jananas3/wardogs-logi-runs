# Wardogs Logi Runs: self-hosted with sync

This version runs on your own free hosting (GitHub Pages) and syncs through your own free Firebase project. Each player signs in with Google and sees only their own data, on any device.

Files:
- `index.html`: the tracker
- `config.js`: your Firebase settings go here (sync stays off while it says `null`)
- `firestore.rules`: security rules so each player can only touch their own data

Cost: free. Firebase's free "Spark" plan needs no credit card, and its daily allowance (50,000 reads and 20,000 writes) is far more than a group of friends will use. Each button tap is about one write.

---

## 1. Create the Firebase project (about 5 min)

1. Go to https://console.firebase.google.com and sign in with a Google account.
2. Click **Create a project**, name it (e.g. `wardogs-tracker`) and finish. You can turn Google Analytics off.
3. On the project home page, click the **Web** icon (`</>`) to add a web app. Give it any nickname and skip Firebase Hosting.
4. Firebase shows a `firebaseConfig = { ... }` block. Copy the values into `config.js` so it looks like the example in that file:
   ```js
   export const firebaseConfig = {
     apiKey: "...",
     authDomain: "...",
     projectId: "...",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "..."
   };
   ```
   These values aren't secret, because they only identify your project. The security rules in step 3 are what protect the data.

## 2. Turn on Google sign-in

1. In the left menu: **Build → Authentication → Get started**.
2. On the **Sign-in method** tab, choose **Google**, switch it on, pick a support email and click **Save**.

## 3. Create the database and paste the rules

1. **Build → Firestore Database → Create database**.
2. Pick a location close to you (e.g. `europe-west3 (Frankfurt)`). You can't change it later.
3. Start in **production mode**.
4. Open the **Rules** tab, replace everything with the contents of `firestore.rules` and click **Publish**.

## 4. Put the page on GitHub Pages

1. Create a free account at https://github.com if you don't have one.
2. Create a new **public** repository, e.g. `wardogs-tracker`.
3. Click **Add file → Upload files** and upload `index.html` and your edited `config.js`. Commit.
4. Go to **Settings → Pages**. Under *Build and deployment*, set **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)**, and click **Save**.
5. After a minute your tracker is live at `https://YOUR-USERNAME.github.io/wardogs-tracker/`.

## 5. Allow your site to sign in

1. Back in Firebase: **Authentication → Settings → Authorized domains → Add domain**.
2. Add `YOUR-USERNAME.github.io` (without `https://` or the folder).

Done. Open the link, tap **Sign in with Google to sync** and play. Share the same link with friends. Each of them signs in with their own Google account and gets their own private history.

---

## How sync behaves

- **Works offline first.** Everything is saved in the browser immediately and uploaded when online. The line under the title shows *Synced ✓*, *Saving…* or *Not synced yet*.
- **Switching devices.** When you open or return to the tab, it pulls your latest data. A session in progress carries over, so you can start on PC and keep tapping on your phone.
- **Merging.** Signing in on a browser that already has local sessions adds them to your account and nothing is lost. Deleted sessions stay deleted on all devices.
- **Signing out** leaves a copy in that browser, but stops syncing.

## Moving data from the Claude version

In the Claude-hosted tracker, open **Backup & share → Export JSON file** (or **Copy JSON**). In this version, use **Import JSON file** (or **Paste JSON**). Import only adds sessions you don't already have.

## Updating later

To change the tracker, upload a new `index.html` to the repository. Keep your `config.js`, since your settings live there.
