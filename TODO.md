# Task Manager - Suggestion Visibility Update

## Completed Tasks
- [x] Modified server/routes/suggestions.js to restrict employee suggestions visibility
  - Admins can see all suggestions
  - Employees can only see their own suggestions
  - Other employees cannot see suggestions posted by different employees

## Changes Made
- Updated GET /suggestions endpoint in server/routes/suggestions.js
- Added role-based filtering logic using req.userRole
- Employees now only receive their own suggestions via API

## Testing Completed
- [x] Code review confirms correct role-based filtering logic
- [x] AdminDashboard fetches all suggestions via getSuggestions() API
- [x] EmployeeDashboard filters suggestions to show only user's own suggestions
- [x] API endpoint correctly restricts visibility based on user role
- [x] Suggestion posting functionality remains unchanged
