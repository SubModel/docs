# Create Access Keys

Learn how to create and manage your InstaGen access keys for secure API access.

## Creating Your First Access Key

### Step 1: Navigate to Account Settings
1. Log in to your SubModel.ai account
2. Go to **Account Settings** from the main menu
3. Click on the **InstaGen Access Key** tab

### Step 2: Generate New Key
1. Click **"Add New Access Key"** button
2. Enter a descriptive name for your key
3. Add an optional description explaining its purpose
4. Click **"Create"** to generate the key

### Step 3: Copy and Secure Your Key
- Your new access key will be displayed immediately
- Copy the key to a secure location
- You can always view your keys again in the Account Settings
- Use the copy button for easy copying

## Key Naming Best Practices

### Descriptive Names
- **Purpose-based**: "Production API", "Development Testing"
- **Application-specific**: "Web App Integration", "Mobile App"
- **Environment-based**: "Staging Environment", "Live Production"

### Examples
- `Production-LLM-API`
- `WebApp-Integration`
- `Testing-Environment`
- `Mobile-App-Access`

## Key Descriptions

### What to Include
- **Intended Use**: How the key will be used
- **Application**: Which application or service uses this key
- **Access Level**: Any specific permissions or limitations
- **Contact**: Who to contact if issues arise

### Example Descriptions
```
Production API key for our main web application.
Used by the customer support chatbot.
Rate limited to 1000 requests per hour.
Contact: dev-team@company.com
```

## Security Considerations

### Key Storage
- Store keys securely in environment variables
- Never commit keys to version control
- Use secure secret management systems
- Rotate keys regularly

### Access Control
- Limit key access to necessary personnel
- Monitor key usage for unusual activity
- Disable unused keys immediately
- Use different keys for different environments

## Next Steps

- [Learn Key Management](manage-keys.md)
- [Understand Usage Tracking](../usage/overview.md)
- [Start Using Models](../models/overview.md)
