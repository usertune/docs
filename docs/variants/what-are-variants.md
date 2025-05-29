# What are Variants?

**Variants** are different versions of the same content that can be shown to different users based on targeting rules, user context, or A/B testing scenarios. They enable true personalization by delivering the right content to the right user at the right time.

## 🎯 Understanding Variants

Variants are the heart of personalization in Usertune. While content defines *what* you want to personalize, variants define the *different versions* of that content. Each variant contains:

- **Unique data payload** - The actual content to be delivered
- **Targeting rules** - Who should see this variant
- **Priority settings** - Which variant takes precedence
- **Performance metrics** - How well this variant performs

Think of variants as the different versions of a movie—same story, but different cuts for different audiences:

```
🎬 Movie: "Hero Banner"
├── 🎯 Theatrical Cut (General audience)
├── 🎯 Director's Cut (Film enthusiasts)  
├── 🎯 Extended Edition (Fans)
└── 🎯 TV Version (Broadcast audience)
```

## 🔧 Variant Structure

### Basic Variant Components

```json
{
  "id": "premium-users",
  "name": "Premium User Banner",
  "description": "Special banner for premium subscribers",
  "data": {
    "title": "Welcome Back, VIP!",
    "subtitle": "Exclusive offers await you",
    "background_color": "#gold",
    "cta_text": "View VIP Deals",
    "cta_url": "/premium-deals"
  },
  "targeting_rules": {
    "user_type": "premium",
    "subscription_status": "active"
  },
  "priority": 10,
  "weight": 100,
  "status": "active",
  "created_at": "2025-01-08T10:00:00Z"
}
```

### Key Properties

#### **Variant ID**
- **Unique identifier** within the content item
- **Descriptive naming** - Indicates the variant's purpose
- **Permanent reference** - Used in analytics and debugging

#### **Data Payload**
- **Actual content** - The information delivered to users
- **Flexible structure** - Can be text, JSON, HTML, or media
- **Consistent schema** - Same fields across variants for reliability

#### **Targeting Rules**
- **User conditions** - Who should see this variant
- **Context-aware** - Based on behavior, attributes, or environment
- **Multiple criteria** - Combine different targeting conditions

#### **Priority & Weight**
- **Priority** - Higher numbers take precedence (1-10 scale)
- **Weight** - For A/B testing traffic distribution
- **Fallback logic** - Ensures content is always delivered

## 🎭 Types of Variants

### **Default Variant**
Every content item must have a default variant that serves as the fallback.

```json
{
  "id": "default",
  "name": "Default Hero Banner",
  "data": {
    "title": "Welcome to Our Store",
    "subtitle": "Discover amazing products",
    "cta_text": "Shop Now"
  },
  "targeting_rules": {},
  "priority": 1,
  "is_default": true
}
```

**Characteristics:**
- **Always matches** - No targeting rules, serves everyone
- **Lowest priority** - Only shown when no other variants match
- **Required** - Every content item must have one
- **Safety net** - Ensures users always see something

### **Targeted Variants**
Variants with specific targeting rules for different user segments.

```json
{
  "id": "new-users",
  "name": "First-Time Visitor Banner",
  "data": {
    "title": "Welcome! Get 20% Off",
    "subtitle": "First-time customers save big",
    "cta_text": "Claim Discount",
    "discount_code": "WELCOME20"
  },
  "targeting_rules": {
    "visit_count": 1,
    "user_type": "new"
  },
  "priority": 5
}
```

**Use cases:**
- **User segments** - Premium vs. free users
- **Behavioral targeting** - New vs. returning visitors
- **Geographic targeting** - Region-specific content
- **Device targeting** - Mobile vs. desktop experiences

### **A/B Test Variants**
Multiple variants for testing different approaches.

```json
[
  {
    "id": "test-a-minimal",
    "name": "Minimal Design Test",
    "data": {
      "title": "Shop",
      "style": "minimal",
      "button_color": "blue"
    },
    "targeting_rules": {"ab_test": "design_test"},
    "priority": 5,
    "weight": 50
  },
  {
    "id": "test-b-detailed", 
    "name": "Detailed Design Test",
    "data": {
      "title": "Discover Our Amazing Products",
      "subtitle": "Quality items at great prices",
      "style": "detailed",
      "button_color": "green"
    },
    "targeting_rules": {"ab_test": "design_test"},
    "priority": 5,
    "weight": 50
  }
]
```

**Benefits:**
- **Data-driven decisions** - Test what works best
- **Performance optimization** - Improve conversion rates
- **Risk mitigation** - Test changes before full rollout
- **Continuous improvement** - Always be testing

## 🎛️ Targeting Rules

Targeting rules determine which users see which variants. Rules can be based on various criteria:

### **User Attributes**
Target based on user properties:

```json
{
  "targeting_rules": {
    "user_type": "premium",
    "age_group": "25-34",
    "location": "US",
    "subscription_tier": "gold",
    "language": "en"
  }
}
```

### **Behavioral Data**
Target based on user actions:

```json
{
  "targeting_rules": {
    "visit_count": {"min": 5},
    "last_purchase": {"within_days": 30},
    "cart_value": {"min": 100},
    "browsing_category": "electronics",
    "pages_viewed": {"min": 3}
  }
}
```

### **Contextual Information**
Target based on environment and timing:

```json
{
  "targeting_rules": {
    "device_type": "mobile",
    "time_of_day": {"between": ["09:00", "17:00"]},
    "day_of_week": ["monday", "tuesday", "wednesday"],
    "referrer": "google",
    "utm_campaign": "summer_sale"
  }
}
```

### **Custom Attributes**
Target based on your own business logic:

```json
{
  "targeting_rules": {
    "loyalty_points": {"min": 1000},
    "preferred_brand": "nike",
    "purchase_frequency": "monthly",
    "marketing_segment": "high_value",
    "experiment_group": "beta_testers"
  }
}
```

## ⚖️ Variant Selection Logic

Usertune uses a sophisticated algorithm to select the best variant for each user:

### Selection Process

1. **Rule Evaluation** - Check user context against all targeting rules
2. **Priority Filtering** - Identify all matching variants
3. **Priority Sorting** - Order by priority (highest first)
4. **Weight Distribution** - For equal priority, use traffic weights
5. **Fallback Handling** - Default variant if no matches

### Example Selection

```json
// User context
{
  "user_type": "premium",
  "device_type": "mobile",
  "visit_count": 3,
  "location": "US"
}

// Available variants (selection order)
1. "premium-mobile-us" (priority: 9) ✅ SELECTED
   rules: user_type=premium + device_type=mobile + location=US
   
2. "premium-mobile" (priority: 8) ✅ matches but lower priority
   rules: user_type=premium + device_type=mobile
   
3. "premium-users" (priority: 7) ✅ matches but lower priority
   rules: user_type=premium
   
4. "mobile-users" (priority: 6) ✅ matches but lower priority
   rules: device_type=mobile
   
5. "default" (priority: 1) ✅ fallback
   rules: none
```

### Traffic Distribution

For A/B testing with equal priority, weights determine traffic split:

```json
// 50/50 A/B test
"variant_a": {"priority": 5, "weight": 50}  // Gets 50% of traffic
"variant_b": {"priority": 5, "weight": 50}  // Gets 50% of traffic

// 70/30 split test
"variant_a": {"priority": 5, "weight": 70}  // Gets 70% of traffic  
"variant_b": {"priority": 5, "weight": 30}  // Gets 30% of traffic
```

## 🔗 Variants in API Responses

When you request content, Usertune returns the selected variant:

### Request with User Context

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "user_context": {
         "user_type": "premium",
         "device_type": "mobile",
         "location": "US"
       }
     }' \
     https://api.usertune.io/v1/workspace/ws_abc123/content/hero-banner
```

### Rich Response with Context

```json
{
  "content_slug": "hero-banner",
  "variant": {
    "id": "premium-mobile-us",
    "name": "Premium Mobile US Banner",
    "data": {
      "title": "VIP Mobile Experience",
      "subtitle": "Exclusive US offers",
      "cta_text": "Shop Premium",
      "background_color": "#gold"
    }
  },
  "personalization_context": {
    "rule_matched": "user_type=premium AND device_type=mobile AND location=US",
    "variant_priority": 9,
    "confidence": 0.98,
    "alternatives_considered": [
      "premium-mobile",
      "premium-users", 
      "mobile-users",
      "default"
    ],
    "selection_time_ms": 2.3
  },
  "analytics": {
    "impression_id": "imp_xyz789",
    "test_group": null,
    "experiment_id": null
  }
}
```

## 🎯 Variant Best Practices

### Design Principles

- **Consistent structure** - Keep data schema consistent across variants
- **Meaningful differences** - Each variant should serve a clear purpose
- **Graceful degradation** - Ensure fallbacks work properly
- **Performance optimization** - Keep variant data size reasonable

### Targeting Strategy

- **Start simple** - Begin with basic user type targeting
- **Add complexity gradually** - Layer on more sophisticated rules
- **Avoid over-targeting** - Too specific rules may never match users
- **Monitor performance** - Track which rules are actually being used

### A/B Testing

- **Test one variable** - Isolate what you're testing
- **Run long enough** - Get statistically significant results
- **Document hypotheses** - Know what you're testing and why
- **Act on results** - Implement winning variants permanently

## 🔍 What's Next?

Learn more about variant management:

- [Targeting Rules →](targeting-rules.md) - Advanced targeting strategies *(coming soon)*
- [A/B Testing →](ab-testing.md) - Comprehensive testing guide *(coming soon)*
- [Variant Analytics →](analytics.md) - Performance monitoring *(coming soon)*
- [Best Practices →](best-practices.md) - Optimization tips *(coming soon)*

---

**Need help?** Contact our support team at [customer@usertune.io](mailto:customer@usertune.io) 