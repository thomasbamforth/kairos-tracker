# Firebase Setup Instructions

To make the Kairos Tracker live with shared data across all users, you need to set up Firebase.

## Step 1: Create Firebase Project

1. Go to https://console.firebase.google.com/
2. Click "Add project"
3. Name it: `kairos-tracker`
4. Disable Google Analytics (optional)
5. Click "Create project"

## Step 2: Set up Firestore Database

1. In your Firebase project, click "Firestore Database" in the left menu
2. Click "Create database"
3. Choose "Start in **production mode**" (we'll add security rules later)
4. Select a location (e.g., `us-central`)
5. Click "Enable"

## Step 3: Get Firebase Configuration

1. Click the gear icon ⚙️ next to "Project Overview"
2. Click "Project settings"
3. Scroll down to "Your apps"
4. Click the web icon `</>`
5. Register app name: `kairos-tracker-web`
6. Click "Register app"
7. **Copy the firebaseConfig object** - it looks like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "kairos-tracker.firebaseapp.com",
  projectId: "kairos-tracker",
  storageBucket: "kairos-tracker.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123..."
};
```

## Step 4: Update index.html

Replace the placeholder Firebase config in `index.html` (around line 30) with your actual config from Step 3.

Look for this section:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBxLKqJ8vZ9X_Qm5YrN3wP2tH6jK4sL8mE",  // ← Replace these
  authDomain: "kairos-tracker.firebaseapp.com",
  projectId: "kairos-tracker",
  // ... etc
};
```

## Step 5: Set up Security Rules (Important!)

1. In Firebase Console, go to "Firestore Database"
2. Click the "Rules" tab
3. Replace the rules with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /teams/{teamId} {
      allow read, write: if true;  // Public read/write for now
    }
  }
}
```

4. Click "Publish"

**Note:** These rules allow anyone to read/write. For production, you should add authentication.

## Step 6: Deploy

1. Commit and push your changes to GitHub
2. GitHub Pages will automatically deploy
3. Everyone who visits the URL will see the same data!

## Testing

1. Open the site in two different browsers
2. Add a team in one browser
3. It should appear in the other browser immediately (real-time sync!)

## Optional: Add Authentication

To restrict who can edit data:

1. Enable Authentication in Firebase Console
2. Choose a sign-in method (Google, Email, etc.)
3. Update security rules to require authentication
4. Add login UI to the app

Let me know when you have your Firebase config and I'll update the code!
