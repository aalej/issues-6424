# Repro for #6424

## Versions

firebase-tools: v12.6.1<br>
node: v18.16.1

## Steps to reproduce:

ToDo: In "public/configs.js", set `firebaseConfig` to your Firebase project's config

### Emulator test

1. In "public/configs.js"
   - Set `useEmulator` to `true`
1. Run `firebase emulators:start --project <project_id>`
1. Open "http://127.0.0.1:5000" to access the hosted app
1. Enter an `email` and `password`
1. Click "Sign up" button
   - UI should update to show user info
1. Click "Update user" button
   - Console logs will show that an error was raised

```
Uncaught (in promise) FirebaseError: Firebase: Error (auth/invalid-json-payload-received.-/displayname-must-be-string).
    at createErrorInternal (assert.ts:161:7)
    at _fail (assert.ts:79:17)
    at _performFetchWithErrorHandling (index.ts:200:22)
    at async _logoutIfInvalidated (invalidation.ts:34:22)
    at async updateProfile (profile.ts:34:23)
    at async HTMLButtonElement.updateUser ((index):42:9)
```

### Firebase project test

1. In "public/configs.js"
   - Set `useEmulator` to `false`
1. Run `firebase emulators:start --only hosting --project <project_id>`
1. Open "http://127.0.0.1:5000" to access the hosted app
1. Enter an `email` and `password`
1. Click "Sign up" button
   - UI should update to show user info
1. Click "Update user" button
   - No errors were raised in the console

## Notes

According to the documentation [here](https://firebase.google.com/docs/reference/js/auth.md#updateprofile), `displayName` and `photoURL` for `updateProfile()` could have `null` values.

From what I can observe, passing `null` values for either `displayName` and `photoURL` will cause the value of the parameter to not change.
