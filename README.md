# JamritChores
A small AI project to create a chore application for the JAMRIT!
www.chores.joshuawilde.me

How the app is put together (files, data model, households, security) is
explained in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

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

Then open the site and sign in with Google. Anyone who isn't in a household
yet is asked to create one (naming it, and giving their own name) or to join
one with an invite link.

## Households

Each household has its own board at `households/{id}/board/*` (schedule,
checked, rules, todos, meals). The household document lists its members by
email with a role: `owner` (manages the household), `editor` (can change the
board) or `viewer` (read-only). `firestore.rules` enforces this on the server.

Owners manage the household from the settings menu (tap "The Board", then People):
- **Who shares the chores**: one to four people, each with a name and a colour
  (tap the dot to change it). Link a person to someone's account so their phone
  opens on their chores. Chores, the bottom switcher, the balance bar, stats and
  "take turns" all follow this list.
- **Invite with a link**: pick Can edit or View only, create a link and share
  it. Anyone who opens it can join with that role for 7 days, unless an owner
  cancels it first. Links are stored at `invites/{code}`; a code can be opened
  by someone who knows it, but the codes can't be listed.
- **Add by email**: the person joins by signing in with that Google account.
- Change anyone's role, or remove them.

A household can be opened by up to 8 accounts and always keeps at least one owner. Anyone
can leave, and anyone can start a new household from the settings menu. If
you're in more than one household, the settings menu lets you switch between
them.

Owners can rename the household from the settings menu. The colour theme stays
per device.

Regent Square (`households/home`) was made from the board that came before
households, which is still in the database at `board/*` as a backup. The app
no longer opens it.
