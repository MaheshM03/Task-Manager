# Employee Profile Feature

## Overview
Employees can now add and manage their personal information directly from their dashboard via a dedicated profile section.

## Features Added

### Frontend (React)
1. **Profile Button** - Located in the employee dashboard header
   - Click to toggle profile view/edit mode
   - Styled with gradient background matching the dashboard theme

2. **Profile Modal**
   - **View Mode**: Displays all profile information in a clean card layout
   - **Edit Mode**: Form with fields for personal information
   - Fields included:
     - Name (read-only)
     - Email (read-only)
     - Phone
     - Department
     - Address (textarea)
     - Bio (textarea)

3. **Profile State Management**
   - `showProfile`: Toggle visibility
   - `profileData`: Stores user profile information
   - `isEditingProfile`: Toggle between view/edit modes
   - `profileLoading`: Loading state during API calls

### Backend (Express.js)
1. **New API Endpoint**: `PUT /api/auth/profile`
   - Protected by authentication middleware
   - Updates user profile fields in users.json
   - Accepts: phone, department, address, bio
   - Returns updated user object

2. **User Data Storage**
   - Profile fields are stored in the users.json file
   - Persists across sessions
   - Used for admin employee list display

### API Integration
1. **New Function**: `updateProfile(profileData)` in `src/utils/api.js`
   - Sends profile updates to backend
   - Includes authentication token
   - Error handling with user feedback

## User Flow

### Viewing Profile
1. Click "👨‍💼 Profile" button in dashboard header
2. Profile panel opens showing all information
3. Fields marked as "Not provided" if empty
4. Click "✏️ Edit Profile" to enter edit mode

### Editing Profile
1. In edit mode, all fields become editable (except name and email)
2. Update desired fields:
   - Phone: Enter phone number
   - Department: Enter department name
   - Address: Enter full address
   - Bio: Add personal description
3. Click "💾 Save Profile" to persist changes
4. Loading state shows while saving
5. Profile automatically closes on successful save
6. Error message shown if save fails

## Technical Details

### File Locations
- Component: `task/src/components/EmployeeDashboard.jsx`
- Styling: `task/src/components/EmployeeDashboard.css`
- API Client: `task/src/utils/api.js`
- Backend Route: `server/routes/auth.js`
- Database: `server/db/users.json`

### Database Schema
```json
{
  "_id": "unique-id",
  "name": "Employee Name",
  "email": "employee@example.com",
  "phone": "+1-555-0000",
  "department": "Engineering",
  "address": "123 Main St, City, State",
  "bio": "Personal description",
  "role": "employee",
  "createdAt": "2024-01-01T00:00:00.000Z"
}
```

### CSS Classes
- `.emp-profile-section`: Modal overlay
- `.profile-container`: Main profile panel
- `.profile-header`: Header with close button
- `.profile-form`: Form container in edit mode
- `.profile-view`: View container in view mode
- `.form-group`: Individual form field
- `.profile-item`: Individual profile item in view mode
- `.btn-save-profile`: Save button (primary)
- `.btn-cancel-profile`: Cancel button (secondary)
- `.btn-edit-profile`: Edit button

## Styling
- Color Scheme: Teal/Cyan gradient primary, slate/gray neutral
- Modal Overlay: Semi-transparent dark background
- Responsive: Mobile-friendly with stacked layout on small screens
- Interactive: Hover effects, focus states, loading indicators

## Validation
- All fields are optional (can remain empty)
- Name and email are read-only (cannot be changed by employee)
- Character limits apply based on textarea/input constraints
- Backend validates token and user existence

## Future Enhancements
- Profile picture upload capability
- Phone number formatting
- Address validation with autocomplete
- Toast notifications for success/error
- Undo/discard changes without saving
- Department auto-complete from company directory
