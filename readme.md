# APEX F1

Four-file responsive Formula 1 analytics site: `index.html`, `style.css`, `app.js`, and this readme.

## Included

- Season selector from 1950 through 2026.
- Overview, drivers, teams, race calendar, live session data, championship standings, analytics, circuits, driver comparison, and quiz pages.
- Search, responsive tables, favourite drivers, circuit outlines, and quiz length choices: 5, 10, 15, or all 20 questions.
- The quiz reports the number of correct answers out of the selected question count. Championship point totals in the standings come from the Formula 1 data source.

## Data sources and limits

- Schedules, classifications, and championship standings load from the [Jolpica F1 API](https://api.jolpi.ca/ergast/f1/). The browser caches responses for up to six hours. See [Jolpica terms](https://github.com/jolpica/jolpica-f1/blob/main/TERMS.md): commercial use requires permission.
- Recent session positions use [OpenF1](https://openf1.org/docs/). The site identifies the source session and timestamp; it does not simulate live timing. Historical coverage varies.
- 2026 circuit outlines are from [F1 Track Layouts SVG](https://github.com/MasterPlay007/F1-Track-Layouts-SVG), CC0 1.0. That collection has 23 layouts and does not include Madrid.
- Missing data remains missing. The site does not invent results, standings, lap times, or telemetry.

A local `file://` preview may be restricted by browser CORS and storage rules. Use a local web server or HTTPS hosting for data APIs.

## Optional sign-in

Cross-device favourite sync uses Firebase Authentication and Cloud Firestore. Create a Firebase project and Web app, enable Email/Password authentication, create Firestore, authorize the hosted domain, and put the Web app's `apiKey`, `authDomain`, `projectId`, and `appId` into `FIREBASE_CONFIG` in `app.js`. Host over HTTPS. Never put service-account credentials in the browser.

Restrict each user to their own `apexUsers/{uid}` document with Firestore rules:

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /apexUsers/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Without Firebase configuration, sign-in shows setup guidance and favourites stay in the current browser.
