# Manage Access Keys

Learn how to effectively manage your InstaGen access keys for optimal security and organization.

## Key Management Dashboard

### Viewing Your Keys
- All access keys are displayed in a list format
- Each key shows name, status, description, and usage information
- Keys are sorted by creation date (newest first)
- Status indicators show active/disabled state

### Key Information Display
- **Name**: User-defined identifier
- **Token**: The actual access key (read-only)
- **Status**: Active or disabled toggle
- **Description**: Optional notes about the key
- **Last Used**: Timestamp of most recent usage
- **Usage Count**: Total number of requests made

## Key Operations

### Enable/Disable Keys
- Use the toggle switch to activate or deactivate keys
- Disabled keys cannot be used for API calls
- Useful for temporary access control
- No data loss when disabling keys

### Edit Key Information
1. Click the **Edit** button on any key
2. Modify the name and description
3. Click **Save** to apply changes
4. Token value remains unchanged

### Reset Keys
- Generate a new token value for existing keys
- Useful for security rotation
- Maintains the same name and description
- Old token becomes invalid immediately

### Delete Keys
- Permanently remove unused keys
- Cannot be undone
- Usage history is preserved for billing
- Confirm deletion in the popup dialog

## Security Best Practices

### Regular Key Rotation
- Rotate keys every 90 days
- Use the reset function for new tokens
- Update applications with new keys
- Monitor for failed authentication attempts

### Access Monitoring
- Review usage patterns regularly
- Check for unusual activity
- Monitor key usage counts
- Verify last used timestamps

### Key Organization
- Use descriptive names for easy identification
- Group keys by purpose or environment
- Maintain clear descriptions
- Archive or delete unused keys

## Troubleshooting

### Common Issues
- **Key Not Working**: Check if key is active
- **Rate Limiting**: Monitor usage patterns
- **Authentication Errors**: Verify key format and status
- **Unexpected Usage**: Review access logs

### Key Recovery
- Disabled keys can be re-enabled
- Reset keys if compromised
- Contact support for account-level issues
- Maintain backup keys for critical applications

## Usage Tracking

### Monitoring Consumption
- Track requests per key
- Monitor token usage patterns
- Analyze cost distribution
- Identify optimization opportunities

### Export and Analysis
- Export usage data for external analysis
- Track key performance over time
- Compare usage across different keys
- Plan capacity and cost management

## Next Steps

- [Create New Access Keys](create-key.md)
- [Monitor Usage Analytics](../usage/overview.md)
- [Learn Security Best Practices](create-key.md#security-considerations)
