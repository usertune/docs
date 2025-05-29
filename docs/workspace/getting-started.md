# Getting Started with Workspaces

This guide will walk you through creating and configuring your first workspace in Usertune.

## 🚀 Creating Your First Workspace

### Step 1: Sign Up

1. Visit [app.usertune.io](https://app.usertune.io)
2. Click **"Sign up"** to create your account
3. Complete the registration form with your details
4. Verify your email address

### Step 2: Onboarding Process

After signing up, you'll be guided through the onboarding process:

1. **Welcome screen** - Introduction to Usertune
2. **Workspace creation** - Your first workspace is created automatically
3. **Basic configuration** - Set your workspace name and preferences
4. **API key generation** - Get your first API credentials

### Step 3: Workspace Configuration

Configure your workspace settings:

```
⚙️ Workspace Settings
├── 📝 Basic Information
│   ├── Workspace Name
│   ├── Description
│   └── Default Language
├── 🌍 Localization
│   ├── Timezone
│   ├── Date Format
│   └── Number Format
├── 🔐 Security
│   ├── API Key Management
│   ├── Access Control
│   └── Audit Settings
└── 💳 Billing
    ├── Subscription Plan
    ├── Usage Limits
    └── Payment Method
```

## 🔑 Getting Your API Keys

### Accessing API Keys

1. **Navigate to Settings** - Click the settings icon in your workspace
2. **Go to API Keys section** - Find the "API Keys" tab
3. **View your keys** - See your generated API keys

### API Key Types

You'll get different keys for different environments:

```bash
# Production API Key (for live applications)
Bearer pk_live_1234567890abcdef...

# Development API Key (for testing)
Bearer pk_dev_1234567890abcdef...
```

### Security Best Practices

- **Never expose API keys** in client-side code
- **Use environment variables** to store keys securely
- **Rotate keys regularly** for security
- **Use different keys** for different environments

## 🧪 Making Your First API Call

Test your workspace connection with a simple API call:

### 1. Create Test Content

First, create a simple content item in your dashboard:

1. Go to **Content** section in your workspace
2. Click **"Create Content"**
3. Set content slug: `welcome-message`
4. Add a default variant with simple text

### 2. Test API Call

```bash
# Replace with your actual workspace ID and API key
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.usertune.io/v1/workspace/ws_your_id/content/welcome-message
```

### 3. Expected Response

```json
{
  "content_slug": "welcome-message",
  "variant": {
    "id": "default",
    "name": "Default Message",
    "data": {
      "text": "Welcome to Usertune!"
    }
  },
  "personalization_context": {
    "rule_matched": "default",
    "confidence": 1.0
  }
}
```

## 📋 Workspace Dashboard Tour

### Navigation Overview

```
🏠 Dashboard
├── 📊 Analytics - Performance metrics and insights
├── 📄 Content - Manage your content items
├── 🎯 Variants - Personalization variants
├── 👥 Team - Team member management
├── 🔧 Settings - Workspace configuration
└── 📚 Documentation - Help and guides
```

### Key Sections

#### **Analytics Dashboard**
- **Content performance** - Which content performs best
- **User engagement** - How users interact with your content
- **Conversion tracking** - Goal completion rates
- **A/B test results** - Performance comparisons

#### **Content Management**
- **Content library** - All your content items
- **Content editor** - Create and edit content
- **Version history** - Track content changes
- **Content organization** - Tags and categories

#### **Variant Management**
- **Variant editor** - Create personalization variants
- **Targeting rules** - Define who sees what content
- **A/B test setup** - Configure content experiments
- **Performance monitoring** - Track variant effectiveness

## 🎯 Quick Setup Checklist

Complete these tasks to get your workspace ready:

### ✅ Basic Setup
- [ ] **Workspace name** - Set a descriptive name
- [ ] **Description** - Add workspace purpose
- [ ] **Timezone** - Set your local timezone
- [ ] **Team invitation** - Invite team members

### ✅ Content Setup
- [ ] **First content item** - Create test content
- [ ] **Default variant** - Add fallback content
- [ ] **API test** - Verify API connectivity
- [ ] **Integration test** - Test with your application

### ✅ Security Setup
- [ ] **API keys** - Secure your API keys
- [ ] **Environment separation** - Use different keys for dev/prod
- [ ] **Access control** - Set appropriate team permissions
- [ ] **Audit logging** - Enable change tracking

## 🛠️ Integration Examples

### Frontend Integration (JavaScript)

```javascript
// Simple fetch example
async function getPersonalizedContent() {
  const response = await fetch(
    'https://api.usertune.io/v1/workspace/ws_your_id/content/welcome-message',
    {
      headers: {
        'Authorization': 'Bearer YOUR_API_KEY',
        'Content-Type': 'application/json'
      }
    }
  );
  
  const content = await response.json();
  document.getElementById('welcome').textContent = content.variant.data.text;
}
```

### Backend Integration (Node.js)

```javascript
const axios = require('axios');

async function getContent(contentSlug, userContext = {}) {
  try {
    const response = await axios.post(
      `https://api.usertune.io/v1/workspace/ws_your_id/content/${contentSlug}`,
      { user_context: userContext },
      {
        headers: {
          'Authorization': 'Bearer YOUR_API_KEY',
          'Content-Type': 'application/json'
        }
      }
    );
    
    return response.data;
  } catch (error) {
    console.error('Failed to fetch content:', error);
    return null;
  }
}
```

## 🔍 What's Next?

Now that your workspace is set up, explore these topics:

- [← What is a Workspace?](what-is-workspace.md) - Learn workspace concepts
- [Team Management →](team-management.md) - Invite and manage team members
- [API Keys →](api-keys.md) - Advanced API key management
- [Best Practices →](best-practices.md) - Workspace organization tips

**Ready to create content?** Check out the [Content Guide](../content/) to start building personalized experiences.

---

**Need help?** Contact our support team at [customer@usertune.io](mailto:customer@usertune.io) 