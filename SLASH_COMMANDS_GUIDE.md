# 🛡️ Security Slash Commands Guide

## Overview

The security system now includes comprehensive slash commands for easy management through Discord's interface. All commands require Administrator permissions.

## Available Slash Commands

### 1. `/security` - Main Security Management

#### Subcommands:

**`/security setup`**
- Interactive setup wizard with menus
- Choose from: Features, Punishments, Limits, Whitelists, Logs
- User-friendly interface for configuration

**`/security status`**
- Shows current security configuration
- Displays enabled/disabled features
- Shows punishment types and violation limits
- Lists logs channel if configured

**`/security enable <feature>`**
- Enables a specific security feature
- Available features:
  - Ban Protection
  - Kick Protection
  - Channel Create Protection
  - Channel Delete Protection
  - Role Create Protection
  - Role Delete Protection
  - Bot Add Protection
  - Dangerous Role Give Protection
  - Vanity Change Protection
  - Server Name Change Protection

**`/security disable <feature>`**
- Disables a specific security feature
- Same feature options as enable

**`/security punishment <feature> <punishment>`**
- Sets punishment type for a feature
- Available punishments:
  - Clear Roles
  - Kick User
  - Ban User
  - Timeout User

**`/security maxviolations <feature> <max_violations>`**
- Sets maximum violations before punishment
- Range: 1-10 violations

**`/security logs <channel>`**
- Sets security logs channel
- All security events will be logged here

### 2. `/securitywhitelist` - Whitelist Management

#### Subcommands:

**`/securitywhitelist add <feature> <type> <target>`**
- Adds member or role to whitelist
- Types: Member, Role
- Target: Mention the user or role

**`/securitywhitelist remove <feature> <target>`**
- Removes member or role from whitelist
- Target: Mention the user or role

**`/securitywhitelist list <feature>`**
- Shows current whitelist for a feature
- Displays both members and roles

**`/securitywhitelist clear <feature>`**
- Clears all whitelists for a feature

### 3. `/securityviolations` - Violation Management

#### Subcommands:

**`/securityviolations view <user> [feature]`**
- Shows violations for a user
- Optional: specific feature to check
- Displays total violations and breakdown

**`/securityviolations reset <user> [feature]`**
- Resets violations for a user
- Optional: specific feature to reset
- If no feature specified, resets all

**`/securityviolations top [limit]`**
- Shows top violators in the server
- Optional: number of users to show (1-10)
- Default: 5 users

## Interactive Setup Wizard

### Starting the Wizard

Use `/security setup` to start the interactive setup wizard. This provides a user-friendly interface for configuring security settings.

### Wizard Options

1. **🔧 Security Features**
   - Enable/disable individual security protections
   - Visual indicators for current status

2. **⚖️ Punishment Types**
   - Choose punishment for violations
   - Clear descriptions of each punishment type

3. **🔢 Violation Limits**
   - Set maximum violations before punishment
   - Quick selection from common values

4. **🛡️ Whitelist Management**
   - Add trusted members/roles
   - View and manage existing whitelists

5. **📝 Logs Configuration**
   - Set up security logging
   - Test logs functionality

## Usage Examples

### Basic Setup

```bash
# Start interactive setup
/security setup

# Enable ban protection
/security enable feature:Ban Protection

# Set punishment to kick
/security punishment feature:Ban Protection punishment:Kick User

# Set max violations to 3
/security maxviolations feature:Ban Protection max_violations:3

# Add trusted role to whitelist
/securitywhitelist add feature:Ban Protection type:Role target:@TrustedRole

# Set logs channel
/security logs channel:#security-logs
```

### Advanced Configuration

```bash
# Enable multiple protections
/security enable feature:Ban Protection
/security enable feature:Kick Protection
/security enable feature:Channel Create Protection

# Set different punishments
/security punishment feature:Ban Protection punishment:Ban User
/security punishment feature:Kick Protection punishment:Kick User
/security punishment feature:Channel Create Protection punishment:Clear Roles

# Set different limits
/security maxviolations feature:Ban Protection max_violations:1
/security maxviolations feature:Kick Protection max_violations:3
/security maxviolations feature:Channel Create Protection max_violations:5
```

### Violation Management

```bash
# Check user violations
/securityviolations view user:@username

# Check specific feature violations
/securityviolations view user:@username feature:Ban Protection

# Reset all violations for user
/securityviolations reset user:@username

# Reset specific feature violations
/securityviolations reset user:@username feature:Ban Protection

# View top violators
/securityviolations top limit:10
```

### Whitelist Management

```bash
# Add member to whitelist
/securitywhitelist add feature:Ban Protection type:Member target:@username

# Add role to whitelist
/securitywhitelist add feature:Ban Protection type:Role target:@ModeratorRole

# View whitelist
/securitywhitelist list feature:Ban Protection

# Remove from whitelist
/securitywhitelist remove feature:Ban Protection target:@username

# Clear all whitelists
/securitywhitelist clear feature:Ban Protection
```

## Command Permissions

All security slash commands require **Administrator** permissions. This ensures only server administrators can modify security settings.

## Best Practices

### 1. Start with Essential Protections
```bash
# Enable core protections first
/security enable feature:Ban Protection
/security enable feature:Kick Protection
/security enable feature:Role Delete Protection
```

### 2. Set Appropriate Punishments
```bash
# Use strict punishments for critical actions
/security punishment feature:Ban Protection punishment:Ban User
/security punishment feature:Role Delete Protection punishment:Ban User

# Use moderate punishments for less critical actions
/security punishment feature:Channel Create Protection punishment:Clear Roles
```

### 3. Configure Whitelists
```bash
# Add trusted roles and members
/securitywhitelist add feature:Ban Protection type:Role target:@AdminRole
/securitywhitelist add feature:Ban Protection type:Member target:@TrustedUser
```

### 4. Set Up Logging
```bash
# Create a security logs channel and set it
/security logs channel:#security-logs
```

### 5. Monitor Violations
```bash
# Regularly check for violations
/securityviolations top limit:10

# Review specific users
/securityviolations view user:@suspicious_user
```

## Troubleshooting

### Common Issues

1. **Commands not appearing**
   - Ensure bot has Administrator permissions
   - Check if slash commands are registered globally
   - Restart the bot to reload commands

2. **Permission errors**
   - Verify bot has necessary permissions
   - Check role hierarchy (bot's role must be above managed roles)

3. **Whitelist not working**
   - Ensure target user/role exists
   - Check if feature is enabled
   - Verify whitelist was added correctly

4. **Violations not tracking**
   - Check if feature is enabled
   - Verify logs channel is set
   - Ensure bot has permissions to view audit logs

### Debug Commands

```bash
# Check current status
/security status

# View all violations
/securityviolations top limit:20

# Check specific user
/securityviolations view user:@username
```

## Security Considerations

1. **Bot Permissions**: Ensure the bot has all necessary permissions
2. **Role Hierarchy**: Bot's highest role must be above roles it manages
3. **Whitelist Management**: Regularly review and update whitelists
4. **Log Monitoring**: Monitor security logs for false positives
5. **Testing**: Test security features in a controlled environment first

## Integration with Events

The slash commands work with the security event system. When events are triggered:

1. **Violation Check**: System checks if user is whitelisted
2. **Violation Recording**: If not whitelisted, violation is recorded
3. **Punishment Application**: If max violations reached, punishment is applied
4. **Logging**: All actions are logged to the configured channel

## Future Enhancements

Planned improvements for the slash command system:

1. **Bulk Operations**: Enable/disable multiple features at once
2. **Preset Configurations**: Quick setup for common security levels
3. **Advanced Filtering**: More detailed violation filtering and reporting
4. **Automated Responses**: Custom automated responses for violations
5. **Integration**: Integration with other moderation systems 