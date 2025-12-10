# Testing Guide - Employee Profile Feature

## Quick Start

### 1. Access the Application
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000/api

### 2. Login as Employee
- Email: `employee@example.com`
- Password: `password123`
- (Or use any registered employee account)

### 3. Test Profile Feature

#### View Profile
1. After logging in, click the **"👨‍💼 Profile"** button in the top-right header
2. A modal panel will open showing your profile information
3. Fields will display either the saved value or "Not provided"

#### Edit Profile
1. With the profile modal open, click **"✏️ Edit Profile"** button
2. The form becomes editable (Name and Email remain read-only)
3. Update fields:
   - **Phone**: Add a phone number (e.g., +1-555-0123)
   - **Department**: Add department (e.g., Engineering, Sales)
   - **Address**: Add address (e.g., 123 Main St, Springfield)
   - **Bio**: Add personal description

#### Save Changes
1. After editing, click **"💾 Save Profile"** button
2. Loading state shows "Saving..." during API call
3. On success, profile modal automatically closes
4. Reopen profile to verify changes were saved

#### Cancel Editing
1. Click **"✕ Cancel"** button while in edit mode
2. Returns to view mode without saving changes

## Expected Behavior

### Frontend
- ✅ Profile button appears in header next to Sign Out
- ✅ Clicking button toggles profile panel visibility
- ✅ Modal has semi-transparent overlay
- ✅ Edit button changes form to editable state
- ✅ Save button calls API and closes modal
- ✅ Name and Email fields are disabled/read-only
- ✅ Loading state prevents duplicate submissions

### Backend
- ✅ API endpoint `/api/auth/profile` accepts PUT requests
- ✅ Requires valid JWT token in Authorization header
- ✅ Updates only phone, department, address, bio fields
- ✅ Persists changes to users.json file
- ✅ Returns updated user object

### Data Persistence
- ✅ Profile data saved to `server/db/users.json`
- ✅ Changes persist across page refreshes
- ✅ Admin dashboard shows updated employee info (name, department, etc.)

## Test Cases

### Test 1: Initial Profile View
1. Login and click profile button
2. Verify all fields display with name and email filled
3. Other fields show "Not provided" if empty

**Expected**: Profile modal opens with current user data

### Test 2: Update Single Field
1. Click "Edit Profile"
2. Enter only phone number
3. Click "Save Profile"
4. Close and reopen profile
5. Verify phone number is saved, others unchanged

**Expected**: Only phone field is updated

### Test 3: Update Multiple Fields
1. Click "Edit Profile"
2. Fill: Phone, Department, Address, Bio
3. Click "Save Profile"
4. Verify all fields saved

**Expected**: All fields persist after save

### Test 4: Cancel Edit
1. Click "Edit Profile"
2. Change some fields
3. Click "Cancel"
4. Verify fields still show old values

**Expected**: Changes are discarded

### Test 5: Close Modal
1. Click Profile button to open
2. Click the "✕" close button
3. Profile button again
4. Verify data persists

**Expected**: Data is preserved between opens

### Test 6: Admin View
1. Login as admin
2. Go to Admin Dashboard
3. View employee list
4. Verify employee shows department if updated

**Expected**: Profile data visible in admin dashboard

## API Testing (Using cURL or Postman)

### Get Token
```bash
POST http://localhost:5000/api/auth/login
Content-Type: application/json

{
  "email": "employee@example.com",
  "password": "password123"
}
```

### Update Profile
```bash
PUT http://localhost:5000/api/auth/profile
Content-Type: application/json
Authorization: Bearer {TOKEN}

{
  "phone": "+1-555-0123",
  "department": "Engineering",
  "address": "123 Main St, City",
  "bio": "Passionate developer"
}
```

## Troubleshooting

### Issue: Profile button not appearing
- Clear browser cache
- Hard refresh page (Ctrl+Shift+R)
- Check console for errors (F12 > Console tab)

### Issue: Changes not saving
- Check network tab (F12 > Network) for API response
- Verify backend server running on :5000
- Check console for error messages

### Issue: Modal not opening
- Check console for JavaScript errors
- Verify showProfile state is toggling
- Test with different account

### Issue: Fields appear blank after refresh
- Verify users.json file exists in server/db/
- Check server console for database errors
- Test API endpoint directly with cURL

## Browser DevTools Tips

### Check Network Requests
1. Open DevTools (F12)
2. Click Network tab
3. Perform profile save
4. Look for PUT request to `/api/auth/profile`
5. Check Response tab for returned data

### Check Console Logs
1. Open Console tab
2. Watch for any error messages
3. Check for profile data logging

### Check Local Storage
1. Application tab
2. Local Storage > http://localhost:3000
3. Look for 'token' to verify authentication
4. Optional: 'employeeProfile' if using localStorage backup

## Success Indicators

✅ Profile modal opens/closes smoothly
✅ Edit mode enables form fields
✅ Save sends API request with correct data
✅ Backend returns success response
✅ Data persists in users.json
✅ Changes visible on profile re-open
✅ No console errors
✅ Loading states display correctly
✅ Responsive on mobile screens
✅ Admin dashboard shows updated info
