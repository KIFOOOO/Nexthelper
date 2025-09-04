# 🛡️ Security System Documentation

## Overview

The security system provides comprehensive protection for Discord servers against various types of attacks and unauthorized actions. It includes protection for bans, kicks, channel/role management, bot additions, and server settings changes.

## Database Structure

### Security Configuration Table (`security_config`)

The main table stores all security settings for each server:

```sql
CREATE TABLE security_config (
  server_id TEXT PRIMARY KEY,
  
  -- Ban protection
  ban_enabled INTEGER DEFAULT 0,
  ban_punishment TEXT DEFAULT 'clear_roles',
  ban_max_violations INTEGER DEFAULT 3,
  ban_whitelisted_members TEXT DEFAULT '',
  ban_whitelisted_roles TEXT DEFAULT '',
  
  -- Kick protection
  kick_enabled INTEGER DEFAULT 0,
  kick_punishment TEXT DEFAULT 'clear_roles',
  kick_max_violations INTEGER DEFAULT 3,
  kick_whitelisted_members TEXT DEFAULT '',
  kick_whitelisted_roles TEXT DEFAULT '',
  
  -- Channel create protection
  channel_create_enabled INTEGER DEFAULT 0,
  channel_create_punishment TEXT DEFAULT 'clear_roles',
  channel_create_max_violations INTEGER DEFAULT 3,
  channel_create_whitelisted_members TEXT DEFAULT '',
  channel_create_whitelisted_roles TEXT DEFAULT '',
  
  -- Channel delete protection
  channel_delete_enabled INTEGER DEFAULT 0,
  channel_delete_punishment TEXT DEFAULT 'clear_roles',
  channel_delete_max_violations INTEGER DEFAULT 3,
  channel_delete_whitelisted_members TEXT DEFAULT '',
  channel_delete_whitelisted_roles TEXT DEFAULT '',
  
  -- Role create protection
  role_create_enabled INTEGER DEFAULT 0,
  role_create_punishment TEXT DEFAULT 'clear_roles',
  role_create_max_violations INTEGER DEFAULT 3,
  role_create_whitelisted_members TEXT DEFAULT '',
  role_create_whitelisted_roles TEXT DEFAULT '',
  
  -- Role delete protection
  role_delete_enabled INTEGER DEFAULT 0,
  role_delete_punishment TEXT DEFAULT 'clear_roles',
  role_delete_max_violations INTEGER DEFAULT 3,
  role_delete_whitelisted_members TEXT DEFAULT '',
  role_delete_whitelisted_roles TEXT DEFAULT '',
  
  -- Bot add protection
  addbot_enabled INTEGER DEFAULT 0,
  addbot_punishment TEXT DEFAULT 'clear_roles',
  addbot_max_violations INTEGER DEFAULT 3,
  addbot_whitelisted_members TEXT DEFAULT '',
  addbot_whitelisted_roles TEXT DEFAULT '',
  
  -- Dangerous role give protection
  dangerous_role_give_enabled INTEGER DEFAULT 0,
  dangerous_role_give_punishment TEXT DEFAULT 'clear_roles',
  dangerous_role_give_max_violations INTEGER DEFAULT 3,
  dangerous_role_give_whitelisted_members TEXT DEFAULT '',
  dangerous_role_give_whitelisted_roles TEXT DEFAULT '',
  
  -- Vanity change protection
  change_vanity_enabled INTEGER DEFAULT 0,
  change_vanity_punishment TEXT DEFAULT 'clear_roles',
  change_vanity_max_violations INTEGER DEFAULT 3,
  change_vanity_whitelisted_members TEXT DEFAULT '',
  change_vanity_whitelisted_roles TEXT DEFAULT '',
  
  -- Server name change protection
  change_server_name_enabled INTEGER DEFAULT 0,
  change_server_name_punishment TEXT DEFAULT 'clear_roles',
  change_server_name_max_violations INTEGER DEFAULT 3,
  change_server_name_whitelisted_members TEXT DEFAULT '',
  change_server_name_whitelisted_roles TEXT DEFAULT '',
  
  -- General settings
  logs_channel TEXT DEFAULT '',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### Security Violations Table (`security_violations`)

Tracks violations for each user:

```sql
CREATE TABLE security_violations (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  server_id TEXT NOT NULL,
  user_id TEXT NOT NULL,
  violation_type TEXT NOT NULL,
  violation_count INTEGER DEFAULT 1,
  last_violation DATETIME DEFAULT CURRENT_TIMESTAMP,
  punishment_applied TEXT DEFAULT '',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(server_id, user_id, violation_type)
);
```

## Protected Features

### 1. Ban Protection
- **Trigger**: When a user is banned
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 2. Kick Protection
- **Trigger**: When a user is kicked
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 3. Channel Create Protection
- **Trigger**: When a new channel is created
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 4. Channel Delete Protection
- **Trigger**: When a channel is deleted
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 5. Role Create Protection
- **Trigger**: When a new role is created
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 6. Role Delete Protection
- **Trigger**: When a role is deleted
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 7. Bot Add Protection
- **Trigger**: When a bot is added to the server
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 8. Dangerous Role Give Protection
- **Trigger**: When a dangerous role is given to a user
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 9. Vanity Change Protection
- **Trigger**: When server vanity URL is changed
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

### 10. Server Name Change Protection
- **Trigger**: When server name is changed
- **Default**: Disabled
- **Punishment**: Clear roles (default)
- **Max Violations**: 3 (default)

## Punishment Types

1. **clear_roles** - Removes all roles from the violator
2. **kick** - Kicks the violator from the server
3. **ban** - Bans the violator from the server
4. **timeout** - Times out the violator (requires duration setting)

## Commands

### Main Security Command: `+security`

#### Subcommands:

1. **`+security status`**
   - Shows current security configuration for the server

2. **`+security enable <feature>`**
   - Enables a specific security feature
   - Example: `+security enable ban`

3. **`+security disable <feature>`**
   - Disables a specific security feature
   - Example: `+security disable kick`

4. **`+security punishment <feature> <type>`**
   - Sets punishment type for a feature
   - Example: `+security punishment ban kick`

5. **`+security maxviolations <feature> <number>`**
   - Sets maximum violations before punishment
   - Example: `+security maxviolations ban 5`

6. **`+security whitelist <feature> <type> <id>`**
   - Adds member or role to whitelist
   - Example: `+security whitelist ban member 123456789`

7. **`+security logs <channel_id>`**
   - Sets channel for security logs
   - Example: `+security logs 987654321`

8. **`+security violations <user_id> [type]`**
   - Shows violations for a user
   - Example: `+security violations 123456789 ban`

9. **`+security reset <user_id> <violation_type>`**
   - Resets violation count for a user
   - Example: `+security reset 123456789 ban`

10. **`+security help`**
    - Shows help information

## Usage Examples

### Basic Setup

1. **Enable ban protection:**
   ```
   +security enable ban
   ```

2. **Set punishment to kick:**
   ```
   +security punishment ban kick
   ```

3. **Set max violations to 5:**
   ```
   +security maxviolations ban 5
   ```

4. **Add trusted role to whitelist:**
   ```
   +security whitelist ban role 123456789
   ```

5. **Set logs channel:**
   ```
   +security logs 987654321
   ```

### Advanced Configuration

**Complete ban protection setup:**
```
+security enable ban
+security punishment ban kick
+security maxviolations ban 3
+security whitelist ban role 123456789
+security whitelist ban member 987654321
```

**Channel protection setup:**
```
+security enable channel_create
+security enable channel_delete
+security punishment channel_create clear_roles
+security punishment channel_delete ban
+security maxviolations channel_create 2
+security maxviolations channel_delete 1
```

## Implementation Notes

### Event Handlers Required

To fully implement the security system, you'll need to create event handlers for:

1. **guildBanAdd** - For ban protection
2. **guildMemberRemove** - For kick protection
3. **channelCreate** - For channel create protection
4. **channelDelete** - For channel delete protection
5. **roleCreate** - For role create protection
6. **roleDelete** - For role delete protection
7. **guildMemberAdd** - For bot add protection
8. **guildMemberUpdate** - For dangerous role give protection
9. **guildUpdate** - For vanity and server name change protection

### Security Utility Functions

The system includes utility functions in `utils/security.js`:

- `getSecurityConfig()` - Get server security config
- `setSecurityConfig()` - Set server security config
- `toggleSecurityFeature()` - Enable/disable features
- `setSecurityPunishment()` - Set punishment type
- `setMaxViolations()` - Set max violations
- `addWhitelistedMembers()` - Add whitelisted members
- `addWhitelistedRoles()` - Add whitelisted roles
- `isWhitelisted()` - Check if user is whitelisted
- `recordViolation()` - Record a security violation
- `getViolationCount()` - Get violation count
- `resetViolationCount()` - Reset violation count
- `setLogsChannel()` - Set logs channel

### Best Practices

1. **Start with essential protections**: Enable ban and kick protection first
2. **Use whitelists**: Add trusted roles and members to whitelists
3. **Set appropriate limits**: Don't set max violations too low
4. **Monitor logs**: Set up a logs channel to monitor security events
5. **Test carefully**: Test security features in a controlled environment first

## Security Considerations

1. **Bot Permissions**: Ensure the bot has necessary permissions to apply punishments
2. **Role Hierarchy**: The bot's highest role must be above roles it needs to manage
3. **Whitelist Management**: Regularly review and update whitelists
4. **Log Monitoring**: Monitor security logs for false positives
5. **Backup Configuration**: Keep backups of security configurations

## Troubleshooting

### Common Issues

1. **Bot can't apply punishments**: Check bot permissions and role hierarchy
2. **False positives**: Add legitimate users/roles to whitelist
3. **Too many violations**: Adjust max violations or punishment type
4. **Logs not working**: Verify channel ID and bot permissions

### Debug Commands

- `+security status` - Check current configuration
- `+security violations <user_id>` - Check user violations
- Check bot logs for error messages

## Future Enhancements

Potential improvements for the security system:

1. **Temporary punishments**: Add timeout duration settings
2. **Custom punishments**: Allow custom punishment actions
3. **Violation decay**: Automatic violation count reduction over time
4. **Advanced whitelisting**: Time-based or conditional whitelists
5. **Security levels**: Different security presets for different server types
6. **Integration**: Integration with other moderation systems
7. **Analytics**: Security event analytics and reporting 