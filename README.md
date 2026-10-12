# JamritChores
A small AI project to create a chore application for the JAMRIT!
www.chores.joshuawilde.me

## Syncing between phones (Firebase)

The Board saves to Firebase so ticks and edits show up on both phones. Until
`firebase-config.js` is filled in, it saves on each device only.

One-time setup in the [Firebase console](https://console.firebase.google.com):

1. **Create a project** (Google Analytics isn't needed).
2. **Add a web app**: Project settings > Your apps > the `</>` icon. Copy the
   `firebaseConfig` object it shows into `firebase-config.js`, replacing `null`.
3. **Turn on Google sign-in**: Build > Authentication > Get started > Sign-in
   method > Google > Enable. Then under Settings > Authorized domains, add
   `chores.joshuawilde.me`.
4. **Create the database**: Build > Firestore Database > Create database
   (production mode, any nearby location).
5. **Paste the rules**: in Firestore > Rules, replace everything with the
   contents of `firestore.rules` and publish.
6. **Add the two of you**: in Firestore > Data, start a collection called
   `members`. Add one document per person whose **Document ID is their Google
   email address** (any field will do, e.g. `name: "Joshua"`).

The first person to sign in uploads the starting schedule; after that both
phones read and write the same board.

Data lives in two Firestore documents: `board/schedule` (the weeks and tasks)
and `board/checked` (which tasks are ticked). The colour theme stays per device.
