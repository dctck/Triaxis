# TRIAXIS Multiplayer - Deployment Guide

## ✅ Your Firebase is Already Configured!

The HTML file now has your Firebase credentials built in.

## 🎮 NEW FEATURES v2.0

### ✨ What's New:
- **Hidden Game Codes**: Game list shows player names, not codes
- **Join by Code**: Manual code entry option
- **Session Persistence**: Survives page refresh!
- **Ready System**: Both players must click "Ready" to start
- **Disconnect Detection**: Alerts when opponent leaves
- **Auto Cleanup**: Abandoned games deleted automatically
- **Reconnect**: Rejoin your game after refresh

## Quick Deploy to GitHub Pages

### Step 1: Create Repository
1. Go to https://github.com/new
2. Name it: `triaxis-multiplayer`
3. Make it **Public**
4. Click "Create repository"

### Step 2: Upload File
1. Click "uploading an existing file"
2. Drag `triaxis-multiplayer.html` 
3. **IMPORTANT**: Rename it to `index.html` (required for GitHub Pages)
4. Click "Commit changes"

### Step 3: Enable GitHub Pages
1. Go to repository Settings
2. Click "Pages" in left sidebar
3. Under "Branch", select `main` and `/root`
4. Click "Save"
5. Wait 1-2 minutes

### Step 4: Play!
Your game will be live at:
```
https://YOUR_USERNAME.github.io/triaxis-multiplayer/
```

## How to Play Multiplayer

### Player 1 (Host):
1. Open the game URL
2. Enter your name
3. Click "Create New Game"
4. Share the **Game Code** with your friend
5. Wait for them to join
6. Click "Ready" when ready to start
7. Game starts when both click Ready!

### Player 2 (Join):
**Option A - From List:**
1. Open the same game URL
2. Enter your name
3. Click on your friend's game in the list
4. Click "Ready"

**Option B - By Code:**
1. Open the same game URL
2. Enter your name
3. Click "Join by Code"
4. Enter the game code
5. Click "Ready"

### Game Features:
- **Refresh Safe**: Page refresh won't lose your game!
- **Disconnect Alert**: You'll know if opponent leaves
- **Turn Indicator**: See whose turn it is
- **Real-time Sync**: All moves update instantly

## Testing Locally

Open `triaxis-multiplayer.html` in your browser - it should work immediately since Firebase is configured!

To test multiplayer:
- Open in Chrome normal window (Player 1)
- Open in Chrome Incognito window (Player 2)
- Both can play the same game

## Firebase Security (Important!)

⚠️ **Current Setup**: Test mode - anyone can read/write (expires in 30 days)

For production, update your Firebase Realtime Database Rules:

1. Go to Firebase Console
2. Realtime Database → Rules
3. Replace with:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

Or for better security:

```json
{
  "rules": {
    "games": {
      ".read": true,
      ".write": true,
      "$gameId": {
        ".indexOn": ["status", "createdAt"],
        "status": {
          ".validate": "newData.val() === 'waiting' || newData.val() === 'ready' || newData.val() === 'active' || newData.val() === 'finished' || newData.val() === 'abandoned'"
        }
      }
    }
  }
}
```

## Troubleshooting

### "Can't create game"
- Check Firebase Console → Realtime Database
- Make sure it's enabled and rules allow write
- Check browser console for errors (F12)

### "Games not showing"
- Click "Refresh Game List" button
- Check that games are in "waiting" status in Firebase
- Old games are auto-cleaned every 5 minutes

### "Disconnected" status
- Check your internet connection
- Verify Firebase URL is correct
- Check Firebase Console for quota limits

### "Lost my game after refresh"
- This shouldn't happen anymore! Session is saved
- Check if you cleared cookies/cache
- Try rejoining using the game code

### "Opponent disconnected but I can't see"
- Refresh the page - disconnect detection should trigger
- If stuck, use "Leave Game" button

## Game Cleanup System

The game automatically:
- ✅ Removes abandoned games (when players leave)
- ✅ Cleans up games older than 1 hour in "waiting" status
- ✅ Runs cleanup every 5 minutes
- ✅ All players see clean game lists

## Features

✅ Multiple concurrent games
✅ Real-time synchronization  
✅ Turn-based gameplay
✅ Ready system (both must ready up)
✅ Connection monitoring
✅ Disconnect detection
✅ Session persistence (survive refresh)
✅ Auto-reconnect
✅ Automatic game cleanup
✅ Join by code or from list
✅ Hidden game codes in list

## Need Help?

Check the browser console (F12) for error messages!

## Known Limitations

- Factory and Trade menus need full implementation
- Energy Tier effects partially implemented
- No undo/rematch feature yet

## Next Steps

To fully complete the game:
1. Implement Factory menu with Firebase updates
2. Implement Trade menu with Firebase updates
3. Add all Energy Tier effects
4. Add game history/statistics
5. Add authentication for better security
