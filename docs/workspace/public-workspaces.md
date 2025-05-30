# Public Workspaces

## Overview

Public workspaces in Usertune allow you to share content with clients and end-users **without requiring authentication**. When a workspace is marked as public, anyone with the proper API endpoints can access its content directly.

## Workspace Features

### Core Attributes

Every workspace has essential attributes that define its behavior and accessibility:

- **Name**: Display name for identifying the workspace (required)
- **Description**: Optional text describing the workspace purpose
- **is_public**: Boolean flag that controls access (default: `false`)
- **Owner**: User who created and manages the workspace

### Making a Workspace Public

A workspace becomes public when the `is_public` field is set to `true`. This can be configured when creating a new workspace or by updating an existing one through the workspace settings.

**Example Workspace Configuration:**
- **Name**: "Marketing Campaign 2024"
- **Description**: "Personalized content for Q1 marketing push"
- **Public Access**: Enabled
- **Owner**: Your user account

### Access Control Comparison

| Feature | Private Workspace | Public Workspace |
|---------|------------------|-------------------|
| **Authentication** | Required | Not required |
| **API Access** | Authenticated users only | Anyone with endpoint |
| **Workspace Settings** | Private to owner | Private to owner |
| **Content Access** | Private | Public |
| **Analytics** | Private to owner | Private to owner |

## How Public Access Works

### API Endpoints

Public workspaces use the same API endpoints but bypass authentication when the workspace is public:

```
# Private workspace (requires auth)
GET /v1/workspace/{workspace_id}/content/{content_slug}
Authorization: Bearer {token}

# Public workspace (no auth required)  
GET /v1/workspace/{workspace_id}/content/{content_slug}
# No Authorization header needed when workspace is public
```

### Example API Call

```javascript
// Simple example - no authentication needed for public workspace
fetch('https://api.usertune.io/v1/workspace/123/content/hero-banner')
  .then(response => response.json())
  .then(data => {
    console.log('Personalized content:', data.content);
  });
```

## Use Cases for Public Workspaces

### 1. Website Integration
Perfect for public websites where you want personalized content without user login requirements.

### 2. Mobile Applications  
Ideal for mobile apps that need personalized content without requiring user accounts.

### 3. Content Syndication
Share personalized content across multiple domains or partner sites.

### 4. Marketing Campaigns
Create personalized landing pages accessible to anyone with the link.

### 5. Developer Testing
Allow team members and clients to test content without authentication setup.

## Security Considerations

### What Remains Protected
- **Workspace Settings**: Name, description, and configuration remain private to the owner
- **Analytics Data**: Usage statistics and performance metrics are still protected  
- **User Information**: No exposure of workspace owner or user data
- **Management Operations**: Only owners can edit, delete, or modify workspace settings

### What Becomes Accessible
- **Content**: All content items within the workspace become publicly readable
- **Variants**: A/B testing configurations become visible through API responses
- **Templates**: Content templates and their structure are exposed

### Best Practices

1. **Content Review**: Regularly audit public workspace content for sensitive information
2. **Naming Convention**: Use clear, descriptive names for public workspaces
3. **Documentation**: Maintain descriptions that explain the workspace purpose
4. **Access Monitoring**: Track usage patterns for public workspaces

## Workspace Management

### Creating Public Workspaces

When creating a new workspace, you can set it as public from the beginning by enabling the "Public Access" option. This is useful for:

- Demo environments
- Marketing content
- Public-facing personalization
- Developer testing scenarios

### Converting Existing Workspaces

You can convert private workspaces to public (or vice versa) at any time through the workspace settings. Consider the security implications before making this change.

### Workspace Configuration

When setting up workspaces, consider these aspects:

- **Name**: Choose descriptive names that clearly identify the workspace purpose
- **Description**: Provide detailed explanations of what the workspace contains
- **Public Setting**: Carefully consider whether content should be publicly accessible
- **Content Organization**: Plan your content structure before making workspaces public

## Monitoring Public Workspaces

### Key Metrics to Track
- **Access Patterns**: Who's accessing your public workspace content
- **Geographic Distribution**: Where requests are coming from
- **Content Performance**: Which content items are most popular
- **Error Rates**: Monitor for unusual access patterns or abuse

### Administrative Oversight
- Regular review of public workspace content
- Monitoring for inappropriate or outdated material
- Performance tracking and optimization
- Security audit of exposed content

---

For detailed information about implementing content delivery and personalization within public workspaces, see the [Content documentation](../content/index.md). 