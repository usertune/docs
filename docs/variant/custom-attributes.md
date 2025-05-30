# Custom Attributes

## Overview

Usertune allows you to pass custom attributes through query parameters in your content requests. These attributes become available as `attribute.*` properties and enable powerful personalization based on user context, behavior, preferences, and any custom data your application provides.

## How Custom Attributes Work

Custom attributes are extracted from query parameters in the URL when requesting content from Usertune. Any query parameter automatically becomes an attribute accessible in variant conditions and payloads.

### URL Structure
```
https://api.usertune.io/v1/workspace/{workspace_id}/content/{content_slug}?key1=value1&key2=value2
```

### Attribute Mapping
- Query parameter `user_id=12345` becomes `attribute.user_id` with value `"12345"`
- Query parameter `plan=premium` becomes `attribute.plan` with value `"premium"`
- Query parameter `page=checkout` becomes `attribute.page` with value `"checkout"`

## Accessing Custom Attributes

All custom attributes are accessible using the `attribute.` prefix followed by the query parameter name.

### Example Request
```
GET /v1/workspace/123/content/hero-banner?user_id=12345&plan=premium&page=checkout&source=email
```

### Available Attributes
- `attribute.user_id` → `"12345"`
- `attribute.plan` → `"premium"`
- `attribute.page` → `"checkout"`
- `attribute.source` → `"email"`

## Common Custom Attribute Categories

### User Identification

#### `attribute.user_id`
- **Description**: Unique user identifier
- **Examples**: `"12345"`, `"user_abc123"`, `"guest_temp_456"`
- **Use Cases**: Personalized content, user-specific offers, account-based targeting

```json
{
  "attribute": "attribute.user_id",
  "operator": "equals",
  "values": [{"value": "12345"}]
}
```

#### `attribute.session_id`
- **Description**: Session identifier for tracking user sessions
- **Examples**: `"sess_abc123"`, `"session_temp_789"`
- **Use Cases**: Session-based content, temporary personalization

```json
{
  "welcome": "Welcome back to your session!"
}
```

### User Demographics

#### `attribute.age`
- **Description**: User's age or age group
- **Examples**: `"25"`, `"30-35"`, `"senior"`
- **Use Cases**: Age-appropriate content, demographic targeting

```json
{
  "attribute": "attribute.age",
  "operator": "greater_than",
  "values": [{"value": "18"}]
}
```

#### `attribute.gender`
- **Description**: User's gender preference
- **Examples**: `"male"`, `"female"`, `"other"`, `"prefer_not_to_say"`
- **Use Cases**: Gender-specific product recommendations

```json
{
  "attribute": "attribute.gender",
  "operator": "equals",
  "values": [{"value": "female"}]
}
```

#### `attribute.language`
- **Description**: User's preferred language (custom override)
- **Examples**: `"en"`, `"es"`, `"fr"`, `"de"`
- **Use Cases**: Language-specific content, localization overrides

```json
{
  "greeting": "Hola! Bienvenido a nuestro sitio."
}
```

### Subscription & Account Information

#### `attribute.plan`
- **Description**: User's subscription plan or account type
- **Examples**: `"free"`, `"basic"`, `"premium"`, `"enterprise"`
- **Use Cases**: Plan-specific features, upgrade prompts, exclusive content

```json
{
  "attribute": "attribute.plan",
  "operator": "equals",
  "values": [{"value": "free"}]
}
```

#### `attribute.subscription_status`
- **Description**: Current subscription status
- **Examples**: `"active"`, `"trial"`, `"expired"`, `"cancelled"`
- **Use Cases**: Billing reminders, renewal offers, re-engagement

```json
{
  "attribute": "attribute.subscription_status",
  "operator": "in",
  "values": [{"value": "trial"}, {"value": "expired"}]
}
```

#### `attribute.member_since`
- **Description**: When user became a member
- **Examples**: `"2024-01-15"`, `"2023"`, `"2024-Q1"`
- **Use Cases**: Loyalty rewards, anniversary messages, tenure-based content

```json
{
  "loyalty_message": "Thank you for being with us since {attribute.member_since}!"
}
```

### Behavioral Attributes

#### `attribute.visit_count`
- **Description**: Number of times user has visited
- **Examples**: `"1"`, `"5"`, `"50"`
- **Use Cases**: First-time visitor content, repeat visitor rewards

```json
{
  "attribute": "attribute.visit_count",
  "operator": "equals",
  "values": [{"value": "1"}]
}
```

#### `attribute.last_purchase`
- **Description**: Date or recency of last purchase
- **Examples**: `"2024-03-01"`, `"30_days_ago"`, `"never"`
- **Use Cases**: Win-back campaigns, purchase reminders

```json
{
  "attribute": "attribute.last_purchase",
  "operator": "equals",
  "values": [{"value": "never"}]
}
```

#### `attribute.purchase_history`
- **Description**: Purchase behavior indicator
- **Examples**: `"frequent"`, `"occasional"`, `"first_time"`, `"vip"`
- **Use Cases**: Customer segmentation, loyalty programs

```json
{
  "attribute": "attribute.purchase_history",
  "operator": "equals",
  "values": [{"value": "vip"}]
}
```

### Context & Navigation

#### `attribute.page`
- **Description**: Current page or section user is viewing
- **Examples**: `"homepage"`, `"checkout"`, `"product_detail"`, `"pricing"`
- **Use Cases**: Page-specific content, contextual messaging

```json
{
  "attribute": "attribute.page",
  "operator": "equals",
  "values": [{"value": "checkout"}]
}
```

#### `attribute.source`
- **Description**: Traffic source or referrer
- **Examples**: `"google"`, `"email"`, `"social"`, `"direct"`, `"affiliate"`
- **Use Cases**: Source-specific messaging, campaign tracking

```json
{
  "attribute": "attribute.source",
  "operator": "equals",
  "values": [{"value": "email"}]
}
```

#### `attribute.campaign`
- **Description**: Marketing campaign identifier
- **Examples**: `"spring_sale"`, `"black_friday"`, `"newsletter_march"`
- **Use Cases**: Campaign-specific content, offer targeting

```json
{
  "campaign_message": "Welcome from our {attribute.campaign} campaign!"
}
```

### E-commerce Attributes

#### `attribute.cart_value`
- **Description**: Current cart value or total
- **Examples**: `"0"`, `"150.99"`, `"500"`
- **Use Cases**: Minimum order promotions, free shipping thresholds

```json
{
  "attribute": "attribute.cart_value",
  "operator": "greater_than",
  "values": [{"value": "100"}]
}
```

#### `attribute.cart_items`
- **Description**: Number of items in cart
- **Examples**: `"0"`, `"3"`, `"10"`
- **Use Cases**: Cart abandonment, bulk purchase incentives

```json
{
  "attribute": "attribute.cart_items",
  "operator": "equals",
  "values": [{"value": "0"}]
}
```

#### `attribute.category_interest`
- **Description**: Product category user is interested in
- **Examples**: `"electronics"`, `"clothing"`, `"books"`, `"home"`
- **Use Cases**: Category-specific recommendations, targeted offers

```json
{
  "attribute": "attribute.category_interest",
  "operator": "in",
  "values": [{"value": "electronics"}, {"value": "gadgets"}]
}
```

### Preferences & Settings

#### `attribute.theme`
- **Description**: User's preferred theme or appearance
- **Examples**: `"light"`, `"dark"`, `"auto"`
- **Use Cases**: Theme-consistent content, design preferences

```json
{
  "attribute": "attribute.theme",
  "operator": "equals",
  "values": [{"value": "dark"}]
}
```

#### `attribute.notifications`
- **Description**: User's notification preferences
- **Examples**: `"enabled"`, `"disabled"`, `"email_only"`
- **Use Cases**: Notification-specific content, permission requests

```json
{
  "attribute": "attribute.notifications",
  "operator": "equals",
  "values": [{"value": "disabled"}]
}
```

#### `attribute.currency`
- **Description**: User's preferred currency
- **Examples**: `"USD"`, `"EUR"`, `"GBP"`, `"JPY"`
- **Use Cases**: Currency-specific pricing, regional offers

```json
{
  "price_display": "Price: {attribute.currency === 'EUR' ? '€99' : '$99'}"
}
```

## Usage Examples

### User Segmentation

#### Premium Users
```json
// Condition for premium users
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.plan",
        "operator": "in",
        "values": [{"value": "premium"}, {"value": "enterprise"}]
      }
    ]
  }
]

// Premium user payload
{
  "title": "Premium Member Benefits",
  "features": ["Priority Support", "Advanced Analytics", "Custom Integrations"],
  "upgrade_cta": null
}
```

#### New Users
```json
// Condition for first-time visitors
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.visit_count",
        "operator": "equals",
        "values": [{"value": "1"}]
      }
    ]
  }
]

// New user onboarding payload
{
  "welcome": "Welcome to our platform! 👋",
  "onboarding_flow": true,
  "special_offer": "Get 20% off your first purchase",
  "tutorial_link": "/getting-started"
}
```

### E-commerce Personalization

#### Cart Abandonment
```json
// Condition for users with items in cart
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.cart_items",
        "operator": "greater_than",
        "values": [{"value": "0"}]
      },
      {
        "attribute": "attribute.page",
        "operator": "not_equals",
        "values": [{"value": "checkout"}]
      }
    ]
  }
]

// Cart reminder payload
{
  "reminder": "You have {attribute.cart_items} items waiting in your cart",
  "cta": "Complete Your Purchase",
  "incentive": "Free shipping on orders over $50"
}
```

#### High-Value Customers
```json
// Condition for high-value cart
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.cart_value",
        "operator": "greater_than",
        "values": [{"value": "200"}]
      }
    ]
  }
]

// VIP treatment payload
{
  "vip_message": "Thank you for your ${attribute.cart_value} order!",
  "free_shipping": true,
  "priority_processing": true,
  "bonus_points": "500"
}
```

### Campaign-Specific Content

#### Email Campaign Visitors
```json
// Condition for email traffic
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.source",
        "operator": "equals",
        "values": [{"value": "email"}]
      }
    ]
  }
]

// Email-specific payload
{
  "headline": "Thanks for clicking from our email!",
  "email_exclusive": "20% off as promised in our newsletter",
  "tracking": "email_conversion"
}
```

#### Referral Traffic
```json
// Condition for referral traffic
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.source",
        "operator": "in",
        "values": [{"value": "partner"}, {"value": "affiliate"}]
      }
    ]
  }
]

// Referral payload
{
  "partner_welcome": "Welcome from our partner!",
  "special_rate": "Partner exclusive: 15% discount",
  "referral_credit": "Your referrer gets $10 credit"
}
```

## Dynamic Content with Custom Attributes

### Personalized Messaging
```json
{
  "welcome": "Hello {attribute.user_id ? 'back' : 'there'}!",
  "plan_status": "You're on our {attribute.plan || 'free'} plan",
  "location_context": "Currently viewing: {attribute.page}",
  "source_acknowledgment": "Thanks for visiting from {attribute.source || 'direct'}!"
}
```

### Conditional Content
```json
{
  "upgrade_prompt": "{attribute.plan === 'free' ? 'Upgrade to Premium for more features!' : null}",
  "cart_status": "{attribute.cart_items > 0 ? 'Cart: ' + attribute.cart_items + ' items' : 'Your cart is empty'}",
  "loyalty_badge": "{attribute.member_since ? 'Member since ' + attribute.member_since : 'New member'}"
}
```

### Progressive Enhancement
```json
{
  "basic_content": "Welcome to our site",
  "enhanced_content": "{attribute.user_id ? 'Welcome back, valued customer!' : 'Join thousands of happy customers'}",
  "premium_features": "{attribute.plan === 'premium' ? ['Advanced Analytics', 'Priority Support'] : ['Basic Features']}"
}
```

## Advanced Custom Attribute Patterns

### Multi-Attribute Targeting
```json
// Complex user segment
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.plan",
        "operator": "equals",
        "values": [{"value": "premium"}]
      },
      {
        "attribute": "attribute.visit_count",
        "operator": "greater_than",
        "values": [{"value": "10"}]
      },
      {
        "attribute": "attribute.last_purchase",
        "operator": "not_equals",
        "values": [{"value": "never"}]
      }
    ]
  }
]
```

### Behavioral Scoring
```json
// High-engagement users
[
  {
    "logic": "or",
    "rules": [
      {
        "attribute": "attribute.visit_count",
        "operator": "greater_than",
        "values": [{"value": "20"}]
      },
      {
        "attribute": "attribute.plan",
        "operator": "in",
        "values": [{"value": "premium"}, {"value": "enterprise"}]
      },
      {
        "attribute": "attribute.purchase_history",
        "operator": "equals",
        "values": [{"value": "frequent"}]
      }
    ]
  }
]
```

### Contextual Combinations
```json
// Mobile checkout abandonment
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.is_mobile",
        "operator": "is_true",
        "values": []
      },
      {
        "attribute": "attribute.page",
        "operator": "equals",
        "values": [{"value": "checkout"}]
      },
      {
        "attribute": "attribute.cart_value",
        "operator": "greater_than",
        "values": [{"value": "50"}]
      }
    ]
  }
]
```

## Best Practices

### 1. Attribute Naming Conventions
- Use consistent naming: `user_id`, `plan_type`, `cart_value`
- Use underscores for multi-word attributes
- Keep names descriptive but concise
- Avoid special characters and spaces

### 2. Data Types and Formats
- All attribute values are strings
- Use consistent formats for dates: `"2024-03-15"`
- Use lowercase for boolean-like values: `"true"`, `"false"`
- Use consistent enumeration values: `"premium"`, not `"Premium"`

### 3. Privacy and Security
- Avoid passing sensitive data in URLs
- Use hashed or anonymized identifiers
- Consider data privacy regulations (GDPR, CCPA)
- Implement proper data retention policies

### 4. Performance Considerations
- Minimize the number of query parameters
- Use efficient data types and formats
- Consider caching implications
- Validate attribute values before use

## Testing Custom Attributes

### Dashboard Testing
In the Usertune dashboard, you can test custom attributes by providing them in the testing interface:

```json
{
  "attribute": {
    "user_id": "12345",
    "plan": "premium",
    "visit_count": "5",
    "cart_value": "150.99",
    "page": "checkout",
    "source": "email"
  }
}
```

### API Testing
Test with real URL parameters:

```bash
# Test with curl
curl "https://api.usertune.io/v1/workspace/123/content/banner?user_id=12345&plan=premium&page=checkout"

# Test different user segments
curl "https://api.usertune.io/v1/workspace/123/content/banner?user_id=67890&plan=free&visit_count=1"
```

### JavaScript Testing
```javascript
// Dynamic URL construction
const attributes = {
  user_id: '12345',
  plan: 'premium',
  page: 'homepage',
  source: 'organic'
};

const queryString = new URLSearchParams(attributes).toString();
const url = `https://api.usertune.io/v1/workspace/123/content/hero?${queryString}`;

fetch(url)
  .then(response => response.json())
  .then(data => console.log(data));
```

## Integration Examples

### JavaScript/React Integration
```javascript
// User context provider
const userContext = {
  userId: user.id,
  plan: user.subscription.plan,
  visitCount: analytics.getVisitCount(),
  cartValue: cart.total,
  page: window.location.pathname
};

// Build content request
const getUserContent = async (contentSlug, context) => {
  const params = new URLSearchParams({
    user_id: context.userId,
    plan: context.plan,
    visit_count: context.visitCount.toString(),
    cart_value: context.cartValue.toString(),
    page: context.page.replace('/', '')
  });

  const response = await fetch(
    `https://api.usertune.io/v1/workspace/123/content/${contentSlug}?${params}`
  );
  
  return response.json();
};
```

### Server-Side Integration (Node.js)
```javascript
// Express middleware for adding user context
app.use((req, res, next) => {
  if (req.user) {
    req.usertuneParams = {
      user_id: req.user.id,
      plan: req.user.plan,
      visit_count: req.session.visitCount || 1,
      member_since: req.user.createdAt.toISOString().split('T')[0]
    };
  }
  next();
});

// Content fetching with user context
const fetchContent = async (contentSlug, userParams) => {
  const url = new URL(`https://api.usertune.io/v1/workspace/123/content/${contentSlug}`);
  
  Object.entries(userParams).forEach(([key, value]) => {
    url.searchParams.append(key, value);
  });

  const response = await fetch(url);
  return response.json();
};
```

## Limitations and Notes

### URL Length Limits
- URLs have practical length limits (usually 2048 characters)
- Consider using essential attributes only
- Use short, meaningful attribute names
- Avoid very long values in query parameters

### Data Type Handling
- All query parameters are strings
- Numeric comparisons work with string representations
- Boolean values should be `"true"` or `"false"`
- Date comparisons work with ISO format strings

### Caching Considerations
- Custom attributes affect cache keys
- High variability in attributes reduces cache efficiency
- Consider grouping similar attribute values
- Use attribute-specific cache strategies

### Security Notes
- Query parameters are visible in logs and analytics
- Avoid personal identifiable information (PII)
- Use hashed or tokenized identifiers
- Implement proper access controls

---

Custom attributes provide unlimited flexibility for personalizing content based on your application's specific user context and business logic. Use them strategically to create highly targeted and relevant user experiences while maintaining performance and privacy standards. 