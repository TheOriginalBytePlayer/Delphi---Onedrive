# OneDrive Permissions Configuration

## Overview
This application uses Microsoft OneDrive OAuth2 authentication to access user files. The permissions (scopes) have been configured to follow the principle of least privilege.

## Current Permissions (Scopes)

### Files.ReadWrite.AppFolder
- **Purpose**: Provides read and write access to a special app-specific folder in the user's OneDrive
- **Security**: Limited access - the app can only read/write files within its own designated folder, not all user files
- **Folder Location**: The app folder is isolated and specific to this application

### offline_access
- **Purpose**: Allows the app to maintain access when the user is not actively using it
- **Usage**: Enables refresh tokens so users don't need to re-authenticate frequently

### openid
- **Purpose**: Required for OpenID Connect authentication
- **Usage**: Basic authentication protocol requirement

### profile
- **Purpose**: Access to basic profile information
- **Usage**: Allows reading user's display name, given name, and surname

### User.Read
- **Purpose**: Read user's basic profile information via Microsoft Graph
- **Usage**: Retrieves user email, display name, and profile photo

## Previous Permissions (Removed)

The following overly broad permissions have been removed:

- ❌ `files.readwrite.all` - Allowed access to ALL user files (replaced with `Files.ReadWrite.AppFolder`)
- ❌ `mail.readwrite` - Mail access (not used by this application)
- ❌ `mail.send` - Send mail capability (not used by this application)
- ❌ `user.readwrite` - Write access to user profile (replaced with `User.Read`)
- ❌ `wl.offline_access`, `wl.basic`, `wl.signin` - Duplicate/legacy Windows Live scopes

## Impact on Users

### What Changed
- Users will now grant access only to an app-specific folder, not their entire OneDrive
- The application can no longer access files outside its designated folder
- Mail-related permissions are no longer requested since they're not used

### What Stays the Same
- Users can still upload and download files through the application
- Authentication flow remains unchanged
- User profile information (name, email, photo) is still accessible

## Security Benefits

1. **Principle of Least Privilege**: App only requests permissions it actually needs
2. **Reduced Attack Surface**: Compromised credentials provide limited access
3. **User Privacy**: User files outside the app folder remain private
4. **Transparent Permissions**: Users see exactly what access they're granting

## Technical Details

The scope configuration is defined in `/OneDrive/uOneDrive.pas` (line 19):
```pascal
scope = 'Files.ReadWrite.AppFolder offline_access openid profile User.Read';
```

## References

- [Microsoft Graph permissions reference](https://docs.microsoft.com/en-us/graph/permissions-reference)
- [Files.ReadWrite.AppFolder documentation](https://docs.microsoft.com/en-us/graph/permissions-reference#filesreadwriteappfolder)
