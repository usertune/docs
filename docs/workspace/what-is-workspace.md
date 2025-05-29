# What is a Workspace?

A **Workspace** is the top-level organizational unit in Usertune that contains all your content, configurations, and settings for a specific project or application.

## 🏢 Understanding Workspaces

Think of a workspace as your project's container—it's where you organize and manage all personalized content for a specific application, website, or product. Each workspace operates independently with its own:

- **Content library** - All your personalized content items
- **API keys and authentication** - Secure access credentials
- **Analytics and performance metrics** - Usage and performance data
- **Team member access controls** - User permissions and roles
- **Billing and usage limits** - Subscription and quota management

## 🔧 Key Features

### **Complete Isolation**
Each workspace is completely isolated from others, ensuring:
- **Data security** - No cross-workspace data access
- **Independent configurations** - Separate settings per workspace
- **Isolated analytics** - Performance metrics per workspace
- **Separate billing** - Usage tracking per workspace

### **Team Collaboration**
Workspaces support collaborative development:
- **Multiple team members** - Invite colleagues to your workspace
- **Role-based permissions** - Admin, Editor, and Viewer roles
- **Audit logging** - Track who changed what and when
- **Collaborative workflows** - Content creation and review processes

### **Environment Support**
Manage different deployment environments:
- **Development environments** - Safe testing without affecting live content
- **Staging environments** - Pre-production testing
- **Production environments** - Live content delivery
- **Environment-specific settings** - Different configurations per environment

## 📋 Workspace Components

### Content Management
```
📄 Content Items
├── hero-banner
├── product-recommendations
├── footer-cta
└── navigation-menu
```

### Security & Access
```
🔑 API Keys
├── Production API Key
├── Development API Key
└── Testing API Key
```

### Team & Permissions
```
👥 Team Members
├── Admin (Full access)
├── Editor (Content management)
└── Viewer (Read-only)
```

### Analytics & Insights
```
📊 Analytics Dashboard
├── Content performance
├── Variant effectiveness
├── User engagement
└── Conversion tracking
```

## 🔗 Workspace Identification

Every workspace has a unique identifier used in API calls:

```bash
# Workspace ID format
workspace_id: ws_abc123

# API endpoint structure
https://api.usertune.io/v1/workspace/{workspace_id}/content/{content_slug}
```

Example API call:
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.usertune.io/v1/workspace/ws_abc123/content/hero-banner
```

## 🎯 Workspace Use Cases

### **Single Project**
One workspace for a single application:
- **E-commerce website** - All product pages and marketing content
- **Mobile app** - In-app messaging and feature flags
- **SaaS platform** - User onboarding and feature announcements

### **Multiple Projects** 
Separate workspaces for different projects:
- **Agency work** - One workspace per client
- **Product suite** - One workspace per product
- **Brand portfolio** - One workspace per brand

### **Environment Separation**
Different workspaces for deployment stages:
- **Development workspace** - Feature development and testing
- **Staging workspace** - QA and integration testing  
- **Production workspace** - Live user-facing content

## 🔍 What's Next?

Learn more about workspace management:

- [Getting Started →](getting-started.md) - Create and configure your first workspace
- [Team Management →](team-management.md) - Invite and manage team members
- [API Keys →](api-keys.md) - Authentication and security
- [Best Practices →](best-practices.md) - Workspace organization tips

---

**Need help?** Contact our support team at [customer@usertune.io](mailto:customer@usertune.io) 