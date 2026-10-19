# Saud Attendence Limited — fresh starter project

A mobile-friendly web app starter with:
- Public account registration, login, and password reset
- Private attendance records per user
- Bulk attendance by date range (Present/Absent/Holiday; Sundays can be excluded)
- Multi-select calendar attendance
- 75% attendance calculator and CSV export
- Public real-time chat for signed-in users
- Notes library with subject filters and admin approval
- Admin moderation for public messages and notes

## Important status
This project is a **starter app**. It becomes an online app only after you connect a Firebase project, publish the Firestore rules, and deploy it to hosting. The provided Firebase config contains placeholders. Do not enter real student records until setup and access rules have been tested.

## 1. Create Firebase project
1. Open https://console.firebase.google.com/ and create a project.
2. Add a Web App and copy its configuration.
3. Open `firebase-config.js` and replace all `YOUR_...` values with the values Firebase gives you.
4. In Firebase Authentication, enable **Email/Password** sign-in.
5. Create a Cloud Firestore database.
6. In Firestore Rules, paste the contents of `firestore.rules` and publish them.

## 2. Make yourself an admin
Public registration intentionally creates only normal `user` accounts. After you create your account and sign in once:
1. Open Firebase Console → Firestore Database → `users` collection.
2. Open the document whose ID is your Firebase Authentication UID.
3. Change its `role` field from `user` to `admin`.
4. Refresh the website and sign in again.

Never give admin access by adding a public registration field. Only do this in the Firebase Console as the project owner.

## 3. Run locally
Because this app uses JavaScript modules, open it through a local web server rather than directly as a `file://` URL. If you have Python installed, run this inside the project folder:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

## 4. Deploy online with Firebase Hosting
1. Install Node.js from https://nodejs.org/.
2. Install Firebase CLI: `npm install -g firebase-tools`
3. In this folder, run `firebase login`.
4. Run `firebase init hosting`, select your existing Firebase project, choose the current folder as the public directory, configure it as a single-page app **No** (this is a multi-page-free static site), and do not overwrite `index.html`.
5. Deploy using `firebase deploy --only hosting`.

Firebase will provide a public HTTPS URL. Keep Firestore rules published. Firebase Hosting and Firestore/Auth have usage limits and pricing conditions; review your project’s plan and usage in the Firebase console.

## Notes on behaviour
- Attendance percentage uses Present / (Present + Absent). Holidays are excluded from the denominator. Sundays are excluded by default during bulk generation, but can be marked manually if needed.
- Bulk attendance has a “Keep existing records” option enabled by default.
- Public notes require admin approval. Private notes are visible only to their author and admin.
- Public chat is visible to signed-in users. Admins can remove messages; users can remove their own messages.
- Notes are text-based in this starter; file uploads are not implemented.
- Chat and notes use Firestore real-time listeners. Large public chat/notes collections may require pagination and/or query indexes as the site grows.

## Security checklist before public launch
- Verify rules in the Firebase Rules Playground and test with two separate accounts.
- Test that account A cannot read account B's attendance path.
- Test that normal users cannot promote themselves to admin.
- Add Firebase App Check, abuse/rate-limit controls, moderation/reporting workflow, and backups before wider public use.
- Do not store sensitive medical/personal data in public chat or notes.
