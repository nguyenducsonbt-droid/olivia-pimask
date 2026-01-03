# Pi Network Integration - Olivia PiMask

Olivia PiMask is fully integrated with Pi Network, providing seamless connectivity and real balance display.

## Features Implemented

### 1. Automatic Pi Account Connection
- **Auto-detection**: Wallet automatically detects when opened in Pi Browser
- **Session Persistence**: Pi account sessions saved to localStorage and IndexedDB
- **Auto-reconnect**: Automatically loads saved sessions on app restart

### 2. Pi Mainnet Balance Display
- **Real Balance**: Fetches actual Pi Mainnet balance using `window.Pi.getBalance()`
- **Live Updates**: Balance refreshes automatically when returning to the app
- **USD Conversion**: Shows balance in USD with 24h price change
- **Refresh Button**: Manual refresh option for instant balance updates

### 3. Pi Network Status Dashboard
Located in Home view and Profile view when connected:
- **Real Pi Balance**: Displays current Mainnet balance
- **Mining Information**: Mining rate and boost percentage
- **KYC Status**: Shows verification status with link to complete KYC
- **Referral Count**: Number of invited pioneers
- **Node Status**: Node operation information if applicable

### 4. Pi Browser Integration
**Header Icon**: Pi logo icon in top-right corner shows:
- Green badge when connected to Pi Mainnet
- Username display on hover
- Click to open Pi Browser menu

**Pi Browser Menu**:
- "Kết nối Pi Mainnet" button - Authenticates with Pi Network
- "Mở trang chủ Pi Network" button - Direct link to mine.pi
- Shows connected username and balance when authenticated

### 5. Security Scan Integration
Security scan checks Pi Mainnet connection:
- **Connected**: Shows 100% security score with username
- **Not Connected**: Shows 80% score with recommendation to connect
- **Real-time Check**: Validates actual Pi session from localStorage

## How It Works

### Connection Flow
1. User clicks "Kết nối Pi Mainnet" button in Pi Browser menu
2. System checks if running in Pi Browser environment
3. Calls `window.Pi.authenticate()` with required scopes
4. Saves session to localStorage and IndexedDB
5. Fetches Pi balance using `window.Pi.getBalance()`
6. Dispatches `olivia-pi-session-changed` event
7. All components update to show connected state

### Balance Synchronization
\`\`\`typescript
// Fetch Pi balance
const balanceResult = await window.Pi.getBalance()
let piBalance = null

if (balanceResult && typeof balanceResult === 'object' && 'balance' in balanceResult) {
  piBalance = balanceResult.balance
} else if (typeof balanceResult === 'number') {
  piBalance = balanceResult
}

// Update balance display
setBalance(piBalance.toString())
\`\`\`

### Session Persistence
\`\`\`typescript
// Save session
await savePiSession({
  user: { username, uid },
  accessToken: token
})

// Load session on app restart
const session = await loadPiSession()
if (session && session.user) {
  setIsConnected(true)
  setUsername(session.user.username)
}
\`\`\`

## User Experience

### When Opening Olivia in Pi Browser
1. App automatically detects Pi Browser environment
2. Loads saved Pi session if exists
3. Displays "Connected to Pi Mainnet" badge with username
4. Shows real Pi balance in balance card
5. Pi Network Status section appears with full details

### When Clicking "Kết nối Pi Mainnet"
1. Button shows "Đang kết nối..." state
2. Pi SDK authentication popup appears
3. User approves scopes (username, payments, wallet_address)
4. Success alert shows: "Kết nối Pi Mainnet thành công! ❤️"
5. Balance syncs automatically
6. All components update to show connected state

### Security Benefits
- Real-time balance verification
- Authentic Pi Network connection
- No manual address entry needed
- Seamless payment integration
- 100% security score when connected

## Components Involved

### Core Components
- `components/pi-browser-menu.tsx` - Pi connection interface
- `components/home-view.tsx` - Balance display and Pi status
- `components/pi-network-status.tsx` - Detailed Pi information
- `components/security-view.tsx` - Security scan with Pi check
- `components/profile-view.tsx` - User profile with Pi status

### Utilities
- `lib/persistent-storage.ts` - Session save/load functions
- `lib/sounds.ts` - Audio feedback
- `lib/haptics.ts` - Vibration feedback

## Storage Keys

### localStorage Keys
- `olivia_wallet_pi_session` - Main Pi session data
- `olivia_wallet_pi_session_backup` - Backup session data
- `pi_wallet_connected` - Connection status flag
- `pi_session_active` - Active session indicator

### IndexedDB
- Database: `OliviaPiMaskDB`
- Store: `piSessions`
- Key: `current`

## Events

### Custom Events
- `olivia-pi-session-changed` - Fired when Pi connection state changes
- `olivia-refresh-ui` - Triggers UI refresh across components
- `change-tab` - Navigation between wallet sections

## Error Handling

### Not in Pi Browser
\`\`\`
"Mở Olivia trong Pi Browser để kết nối Pi Mainnet nhé ❤️"
\`\`\`

### Authentication Failed
\`\`\`
"Không thể kết nối với Pi Network. Vui lòng thử lại."
\`\`\`

### Balance Fetch Failed
- Falls back to showing last known balance
- Allows manual refresh

## Testing Checklist

- [x] Connect Pi account from Pi Browser
- [x] Balance displays correctly
- [x] Session persists after closing app
- [x] Security scan shows 100% when connected
- [x] Pi Network Status shows correct information
- [x] Reconnect works after app restart
- [x] Error handling works when not in Pi Browser
- [x] All components update when connection changes

## Future Enhancements

- Transaction signing with Pi Network
- Pi payment integration for dApps
- Mining history display
- Referral management
- Node configuration interface
