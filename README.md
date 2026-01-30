# 📝 Work Log App

**Version:** 1.0.5  
**Last Updated:** 2026-01-30  
**Authentication:** Google OAuth 2.0

A simple, cloud-synced time tracking application for teams. Track your daily work activities with start/end times, durations, and descriptions across all your devices.

## Version History

### v1.0.0 (2026-01-30)
- Initial release with Google Authentication
- Cloud sync via Firebase Firestore
- Timer and manual time entry
- Date filtering (Today, Week, Month, All Time)
- CSV export for Excel analysis
- Multi-user support with data isolation

## Features

### ⏱️ Time Tracking
- **Timer Function**: Start/stop timer with live countdown
- **Manual Entry**: Backfill activities with custom start/end times
- **Duration Calculation**: Automatic calculation of time spent on tasks
- **Task Descriptions**: Optional detailed notes for each activity

### 📊 Data Management
- **Cloud Sync**: All data synced via Firebase Firestore
- **Historical View**: Filter by Today, This Week, This Month, or All Time
- **Daily Summaries**: Automatic grouping by date with daily totals
- **CSV Export**: Export filtered data to Excel for analysis

### 🔐 Authentication
- **Google Sign-In**: Secure authentication via Google OAuth
- **Multi-User Support**: Each team member has private, isolated data
- **Cross-Device Sync**: Access your logs from any device

## Setup Instructions

### Prerequisites
- Firebase account (free tier)
- Google account (for authentication)
- GitHub account (for hosting)

### 1. Firebase Setup

1. **Create Firebase Project**
   - Go to [Firebase Console](https://console.firebase.google.com)
   - Click "Add project"
   - Name your project (e.g., "work-log-app")
   - Disable Google Analytics (optional)
   - Click "Create project"

2. **Enable Firestore Database**
   - Navigate to "Build" → "Firestore Database"
   - Click "Create database"
   - Start in **production mode**
   - Choose your preferred location
   - Click "Enable"

3. **Configure Firestore Security Rules**
   - Go to Firestore Database → Rules tab
   - Replace the rules with:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /logs/{userId}/entries/{entryId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```
   - Click "Publish"

4. **Enable Authentication**
   - Navigate to "Build" → "Authentication"
   - Click "Get started"
   - Go to "Sign-in method" tab
   - Click on "Google"
   - Toggle **Enable**
   - Enter a project support email (your email address)
   - Click "Save"
   - That's it! Google authentication is now enabled.

5. **Get Firebase Config**
   - Click the gear icon → "Project settings"
   - Scroll to "Your apps" section
   - Click the web icon (`</>`)
   - Register your app
   - Copy the `firebaseConfig` object

### 2. Update Firebase Config in Code

Open `work-log.html` (or `index.html`) and update the `firebaseConfig` object with your values:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. Deploy to GitHub Pages

1. **Create GitHub Repository**
   - Go to [GitHub](https://github.com)
   - Create a new repository (e.g., "work-log")
   - Make it public or private (your choice)

2. **Upload Files**
   - Upload `work-log.html` to your repository
   - Rename it to `index.html`
   - Commit the changes

3. **Enable GitHub Pages**
   - Go to repository Settings → Pages
   - Source: Select your main branch
   - Click "Save"
   - Your app will be live at `https://yourusername.github.io/work-log`

## Usage Guide

### Getting Started

1. **Sign In**
   - Open the app URL
   - Click "Sign in with Google"
   - Select your Google account
   - Grant permissions

2. **Log an Activity**
   - Enter what you're working on
   - Optionally add a description
   - Either:
     - Click "Start Timer" to track in real-time, then "Stop Timer" when done
     - OR manually enter start and end times
   - Click "Log Activity"

### Viewing Logs

- **Filter by Date**: Use the dropdown to view Today, This Week, This Month, or All Time
- **Daily Totals**: Each date shows the total hours worked that day
- **Individual Entries**: See start time, end time, and duration for each task

### Exporting Data

1. Select your desired date filter
2. Click "Export to Excel"
3. A CSV file will download with columns:
   - Date
   - Start Time
   - End Time
   - Duration (minutes)
   - Task
   - Description

4. Open in Microsoft Excel for analysis, pivot tables, charts, etc.

### Tips

- **Backfilling**: Forgot to log something? Just enter the times manually
- **Timer Best Practice**: Start the timer when you begin work for accurate tracking
- **Descriptions**: Add context to help with weekly reviews
- **Regular Exports**: Export weekly/monthly for record-keeping

## Data Structure

### Firestore Collections

```
logs (collection)
  └── {userId} (document)
      └── entries (subcollection)
          └── {entryId} (document)
              ├── task: string
              ├── description: string
              ├── startTime: timestamp (ISO string)
              ├── endTime: timestamp (ISO string)
              ├── duration: number (minutes)
              ├── userEmail: string
              ├── userName: string
              └── createdAt: serverTimestamp
```

### CSV Export Format

When exported, the CSV includes:
- User Email
- User Name
- Date
- Start Time
- End Time
- Duration (minutes)
- Task
- Description

### Security

- Each user can only read/write their own data
- User ID is the Firebase Authentication UID
- Data is isolated per user automatically

## Browser Support

- **Desktop**: Chrome, Firefox, Safari, Edge (latest versions)
- **Mobile**: iOS Safari, Chrome for Android
- **PWA Support**: Add to home screen on iOS/Android for app-like experience

## Troubleshooting

### "Sign in failed" Error
- Check that Google provider is enabled in Firebase Authentication
- Verify authorized domains include your GitHub Pages domain
- Ensure you're using a valid Google account
- Check browser console for detailed error messages

### Data Not Syncing
- Check internet connection
- Verify Firestore security rules are published
- Check browser console for errors

### Can't See Logs on Mobile
- Ensure you're signed in with the same Google account across devices
- Check that the app URL is correct (https, not http)
- Try clearing browser cache and signing in again

### Export Not Working
- Ensure you have logs in the selected date range
- Check that pop-ups are not blocked in your browser
- Try a different browser if issues persist

## Privacy & Security

- **Authentication**: Secured via Google OAuth 2.0
- **Data Storage**: Encrypted in transit and at rest via Firebase
- **Access Control**: Users can only access their own data
- **No Third-Party Tracking**: No analytics or tracking scripts

## Cost Estimate (Firebase Free Tier)

- **Firestore Reads**: 50,000/day (plenty for small teams)
- **Firestore Writes**: 20,000/day
- **Authentication**: Unlimited
- **Hosting**: 10GB/month bandwidth

Typical usage for 10 users logging 20 activities/day:
- Writes: ~200/day (well within free tier)
- Reads: ~2,000/day (well within free tier)

## Updating the App

When a new version is released:

1. **Check Version**: Look at the version number in the footer of the app or in the HTML file header
2. **Backup Data**: Your data is in Firebase, but you can export to CSV as a backup
3. **Update File**: Replace your `index.html` on GitHub with the new version
4. **Clear Cache**: Hard refresh your browser (Ctrl+Shift+R or Cmd+Shift+R)
5. **Test**: Sign in and verify everything works

## Version History

### v1.0.5 (2026-01-30)
- Added user email and name to log entries
- Updated CSV export to include user information
- Switched to popup authentication for better reliability

### v1.0.4 (2026-01-30)
- Switched from redirect to popup sign-in method
- Improved authentication reliability

### v1.0.0 (2026-01-30)
- Initial release with Google Authentication
- Cloud sync via Firebase Firestore
- Timer and manual time entry
- Date filtering (Today, Week, Month, All Time)
- CSV export for Excel analysis
- Multi-user support with data isolation

## License

This project is open source and available for personal and commercial use.

## Support

For issues or questions:
1. Check the Troubleshooting section above
2. Review Firebase Console for errors
3. Check browser developer console for error messages

## Roadmap / Future Enhancements

Potential features for future versions:
- [ ] Team dashboards with aggregated stats
- [ ] Project/client tagging
- [ ] Billable hours tracking
- [ ] Mobile app (React Native)
- [ ] Slack/Teams integration
- [ ] Calendar sync
- [ ] Weekly email summaries
- [ ] Custom reports and analytics

## Credits

Built with:
- Firebase (Authentication & Firestore)
- Microsoft Azure AD
- Vanilla JavaScript (no frameworks)
- GitHub Pages (hosting)
