# Customizable Prefix System

## Overview
The bot now supports customizable prefixes per server, allowing server administrators to set their own prefix instead of using the default `+` prefix.

## Features

### 🔧 **Per-Server Prefixes**
- Each server can have its own unique prefix
- Default prefix is `+` if no custom prefix is set
- Prefixes are stored in the database and persist across bot restarts

### 👑 **Admin-Only Control**
- Only users with **Administrator** permission can change the prefix
- No need for higher permissions than the bot
- Secure and controlled access

### 🛡️ **Validation & Security**
- Prefix length: 1-5 characters
- Blocks problematic prefixes (mentions, URLs, etc.)
- Validates character types for safety

## Commands

### `[prefix]setprefix [new_prefix]`
**Aliases:** `prefix`, `changeprefix`

**Permissions Required:** Administrator

**Usage Examples:**
```
+setprefix !          # Change prefix to !
+setprefix $          # Change prefix to $
+setprefix bot        # Change prefix to "bot"
+setprefix            # Show current prefix
```

**Features:**
- Shows current prefix when used without arguments
- Validates new prefix for security
- Provides immediate feedback and test command
- Logs who changed the prefix and when

## Database Schema

```sql
CREATE TABLE server_prefixes (
  server_id TEXT PRIMARY KEY,
  prefix TEXT NOT NULL DEFAULT '+',
  set_by TEXT NOT NULL,
  set_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

## Implementation Details

### **Prefix Resolution**
1. Bot checks if message starts with server-specific prefix
2. Falls back to default `+` prefix if no custom prefix is set
3. Works seamlessly with all existing commands

### **Command Processing**
- Dynamic prefix detection in messageCreate event
- All commands automatically work with custom prefixes
- Help command shows correct prefix for each server

### **Security Measures**
- **Length Validation:** 1-5 characters maximum
- **Character Validation:** Only letters, numbers, and common symbols
- **Blacklist:** Blocks mentions (@everyone, @here) and URLs
- **Permission Check:** Administrator permission required

## Benefits

### **Server Customization**
- Each server can have its own unique bot experience
- Reduces conflicts with other bots using the same prefix
- Allows for server-specific branding

### **User Experience**
- Users can choose familiar prefixes
- Reduces confusion with multiple bots
- Maintains command functionality across prefix changes

### **Administrative Control**
- Server admins have full control over their bot prefix
- No need for bot owner intervention
- Secure and permission-based system

## Migration

### **Existing Servers**
- All existing servers automatically use the default `+` prefix
- No action required for current functionality
- Can optionally set custom prefixes at any time

### **New Servers**
- Start with default `+` prefix
- Can immediately set custom prefix if desired
- Full backward compatibility maintained

## Technical Notes

### **Performance**
- Prefix lookups are cached per message
- Minimal database overhead
- Efficient querying with indexed server_id

### **Compatibility**
- Works with all existing commands
- No changes required to command files
- Maintains all current functionality

### **Error Handling**
- Graceful fallback to default prefix on errors
- Comprehensive validation prevents invalid prefixes
- Clear error messages for users

## Usage Examples

### **Setting a Custom Prefix**
```
User: +setprefix !
Bot: ✓ Prefix Updated Successfully
     Server: My Server
     New Prefix: !
     Set By: @User#1234
     Status: ✓ Active
     Test Command: !help
```

### **Checking Current Prefix**
```
User: +setprefix
Bot: Current Server Prefix
     Server: My Server
     Current Prefix: !
     Default Prefix: +
     Usage: !setprefix [new_prefix]
```

### **Using Custom Prefix**
```
User: !help
Bot: [Shows help menu with ! prefix]

User: !ban @user Spam
Bot: [Executes ban command]
```

The customizable prefix system provides flexibility while maintaining security and ease of use for server administrators. 