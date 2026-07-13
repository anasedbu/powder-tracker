# Powder Coating — Live Floor Tracker · Setup

You'll do two things once: (A) create a free Firebase database so everyone sees the floor live,
and (B) put the page on GitHub Pages so it has a free public link. ~10 minutes total, on a computer.

---

## A. Firebase (the live sync + auto-save) — ~5 min

1. Go to **https://console.firebase.google.com** and sign in with your Google account.
2. Click **Add project** → name it e.g. `powder-tracker` → you can **disable** Google Analytics → **Create project**.
3. On the left, click **Build → Firestore Database → Create database**.
   - Choose a location near you (e.g. `eur3` / a Gulf region).
   - Start in **Production mode** → **Enable**.
4. Open the **Rules** tab and paste this, then click **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /jobs/{doc} {
         allow read: if true;      // anyone with the link can VIEW
         allow write: if true;     // the app's PIN controls who edits
       }
     }
   }
   ```
   > Note: the PIN in the app hides edit buttons from viewers. The rules above keep it simple and free.
   > If you later want tighter security, tell me and I'll switch it to Firebase Auth.

5. Click the **gear icon → Project settings**. Scroll to **Your apps** → click the **`</>` (web)** icon.
   - App nickname: `tracker` → **Register app** (skip Hosting).
   - You'll see a `const firebaseConfig = { ... }` block. **Copy the whole `{ ... }` object.**

6. Open **`index.html`** in a text editor. Near the top of the `<script>` find:

   ```js
   const firebaseConfig = {
     apiKey: "PASTE_API_KEY",
     ...
   };
   ```
   Replace that whole object with the one you copied. Save.

7. In the same file, right below it set your secret PIN:

   ```js
   const ADMIN_PIN = "1234";   // change to your own number
   ```

---

## B. GitHub Pages (the free public link) — ~4 min

1. Go to **https://github.com** → **New repository** → name it `powder-tracker` → **Public** → **Create**.
2. Click **uploading an existing file** and drag in **`index.html`** (and `SETUP.md` if you like) → **Commit**.
3. Go to the repo's **Settings → Pages**.
   - **Source:** Deploy from a branch → **Branch:** `main` → **/(root)** → **Save**.
4. Wait ~1 minute, refresh. You'll get a link like:
   **`https://YOURNAME.github.io/powder-tracker/`**

That link is your **live floor board**. Share it on WhatsApp with anyone — they all see updates the second you make them.

---

## Daily use

- Open the link on your phone → tap the **🔒** top-right → enter your PIN → you're now **Admin**.
- Tap the big **➕** to add a job: RAL number (a real colour swatch appears), customer name, optional
  description / quantity / photos (camera or gallery).
- Jobs show on **On the Floor**, sorted earliest → latest, each with a **live ticking timer**.
- Tap a card to expand details + photos.
- Tap **✓ Mark Done** when finished → it moves to **Completed** automatically and is saved forever.
- In **Completed**, use the search box to look up any old job by **customer name or RAL number**.
- Everyone else just opens the link — no PIN — and watches live, view-only.

## Notes
- Photos are auto-compressed on your phone before saving, so it stays fast and within the free tier.
- To change the PIN later, edit `ADMIN_PIN` in `index.html` and re-upload it to GitHub.
- Free-tier limits are far beyond a single plant's daily volume.
