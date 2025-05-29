# What is Content?

**Content** in Usertune represents individual pieces of information that can be personalized and delivered to your users. Each content item is a container for different variations (variants) of the same conceptual piece of content.

## 📄 Understanding Content

Content is the fundamental unit of personalization in Usertune. It represents any piece of information you want to deliver dynamically to your users—from simple text snippets to complex data structures like product recommendations, banners, or entire page sections.

### Content as a Container

Think of content as a container that holds multiple versions (variants) of the same conceptual information:

```
📄 Content: "hero-banner"
├── 🎯 Default variant (Standard welcome message)
├── 🎯 New user variant (First-time visitor special offer)
├── 🎯 Premium variant (VIP member exclusive content)
└── 🎯 Mobile variant (Mobile-optimized layout)
```

## 🔧 Content Structure

### Basic Components

Every content item consists of:

```json
{
  "content_slug": "hero-banner",
  "title": "Homepage Hero Banner",
  "description": "Main banner for homepage personalization",
  "type": "banner",
  "variants": [
    {
      "id": "default",
      "name": "Default Banner",
      "data": { ... }
    },
    {
      "id": "premium-users",
      "name": "Premium User Banner", 
      "data": { ... }
    }
  ],
  "created_at": "2025-01-08T10:00:00Z",
  "updated_at": "2025-01-08T15:30:00Z"
}
```

### Content Slug

The **content slug** is the unique identifier for your content:

- **Unique within workspace** - No two content items can have the same slug
- **URL-friendly format** - Lowercase letters, numbers, and hyphens only
- **Permanent reference** - Used in all API calls
- **Descriptive naming** - Should indicate the content's purpose

#### Good Slug Examples
```
hero-banner
product-recommendations
checkout-cta
navigation-menu
footer-links
pricing-table
testimonials-section
```

#### Poor Slug Examples
```
content-1        # Not descriptive
Hero Banner      # Contains spaces and capitals
hero_banner      # Uses underscores (prefer hyphens)
temp-content     # Temporary naming
```

### Content Metadata

Additional information that helps organize and manage content:

#### **Title**
Human-readable name displayed in the dashboard
```json
{
  "title": "Homepage Hero Banner"
}
```

#### **Description**
Detailed explanation of the content's purpose
```json
{
  "description": "Main banner displayed on homepage for user personalization based on subscription status and device type"
}
```

#### **Type**
Category for organization and filtering
```json
{
  "type": "banner" // banner, text, json, media, etc.
}
```

#### **Tags**
Custom labels for filtering and organization
```json
{
  "tags": ["homepage", "marketing", "mobile-optimized"]
}
```

## 🎯 Content Types

Usertune supports various content types to fit your needs:

### **Text Content**
Simple text snippets for headlines, descriptions, or labels.

```json
{
  "type": "text",
  "variant": {
    "data": {
      "text": "Welcome to our exclusive sale!",
      "format": "plain"
    }
  }
}
```

**Use cases**: Headlines, button labels, short descriptions, alerts

### **Rich Content (HTML)**
HTML content with formatting and styling.

```json
{
  "type": "html", 
  "variant": {
    "data": {
      "html": "<h2>Special Offer</h2><p>Save 20% today!</p>",
      "css": "banner-style"
    }
  }
}
```

**Use cases**: Formatted text, banners, rich descriptions, email templates

### **Structured Data (JSON)**
Complex JSON objects for dynamic content.

```json
{
  "type": "json",
  "variant": {
    "data": {
      "title": "Featured Products",
      "products": [
        {"id": 123, "name": "Laptop", "price": 999},
        {"id": 456, "name": "Mouse", "price": 29}
      ],
      "layout": "grid",
      "show_prices": true
    }
  }
}
```

**Use cases**: Product listings, navigation menus, complex configurations, API responses

### **Media Content**
Images, videos, and other media assets.

```json
{
  "type": "media",
  "variant": {
    "data": {
      "image_url": "https://cdn.example.com/banner.jpg",
      "alt_text": "Summer sale banner",
      "width": 1200,
      "height": 400,
      "format": "webp"
    }
  }
}
```

**Use cases**: Images, videos, audio files, downloadable assets

## 📋 Content Organization

### Logical Grouping

Organize content by application area or page:

```
📁 E-commerce Website
├── 🏠 Homepage
│   ├── hero-banner
│   ├── featured-products
│   ├── testimonials
│   └── newsletter-signup
├── 🛒 Product Pages
│   ├── product-recommendations
│   ├── price-display
│   ├── add-to-cart-cta
│   └── customer-reviews
├── ✅ Checkout
│   ├── checkout-form
│   ├── payment-options
│   ├── shipping-info
│   └── order-confirmation
└── 👤 Account
    ├── welcome-message
    ├── account-navigation
    └── subscription-offers
```

### Naming Conventions

Use consistent, descriptive naming patterns:

#### **Page-Component Pattern**
```
{page}-{component}
homepage-hero
product-recommendations  
checkout-button
```

#### **Context-Component-Purpose Pattern**
```
{context}-{component}-{purpose}
mobile-navigation-menu
desktop-sidebar-ads
email-newsletter-cta
```

#### **Feature-Based Pattern**
```
{feature}-{element}
onboarding-welcome
subscription-upgrade-prompt
cart-abandonment-reminder
```

## 🔗 Content in API Calls

### Basic Content Retrieval

```bash
# Get content without user context (returns default variant)
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.usertune.io/v1/workspace/ws_abc123/content/hero-banner
```

### Personalized Content Retrieval

```bash
# Get content with user context for personalization
curl -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "user_context": {
         "user_type": "premium",
         "device_type": "mobile"
       }
     }' \
     https://api.usertune.io/v1/workspace/ws_abc123/content/hero-banner
```

### API Response Structure

```json
{
  "content_slug": "hero-banner",
  "variant": {
    "id": "premium-mobile",
    "name": "Premium Mobile Banner",
    "data": {
      "title": "VIP Mobile Experience",
      "subtitle": "Exclusive offers on your device",
      "cta_text": "View Premium Deals",
      "background_color": "#gold"
    }
  },
  "personalization_context": {
    "rule_matched": "user_type=premium AND device_type=mobile",
    "confidence": 0.95
  }
}
```

## 🎯 Content Lifecycle

### Creation Process

1. **Plan structure** - Decide what to personalize
2. **Choose slug** - Pick a unique, descriptive identifier
3. **Define type** - Select appropriate content type
4. **Create variants** - Build different versions
5. **Test integration** - Verify API connectivity

### Management Workflow

```
🔄 Content States
├── 📝 Draft → Content being created
├── 👀 Review → Pending approval
├── ✅ Active → Live and serving users
├── 📊 Testing → A/B test running
├── 🔄 Updating → Changes in progress
└── 📚 Archived → No longer used
```

## 🔍 What's Next?

Learn more about content management:

- [Content Types →](content-types.md) - Deep dive into different content formats
- [Managing Content →](managing-content.md) - Creation, editing, and organization
- [Content API →](api-reference.md) - Complete API documentation
- [Best Practices →](best-practices.md) - Optimization and performance tips

---

**Need help?** Contact our support team at [customer@usertune.io](mailto:customer@usertune.io) 