# Variant Payload

## Overview

The **Payload** is the actual content data that users receive when a variant is selected. It's a JSON object containing key-value pairs that define what information will be delivered to your application.

## Payload Structure

### Basic Format
Payloads are JSON objects with string keys and values:

```json
{
  "key1": "value1",
  "key2": "value2",
  "key3": "value3"
}
```

### Key Naming Conventions
- Use descriptive, lowercase keys with underscores
- Examples: `welcome_message`, `cta_button`, `banner_text`, `promotion_title`

### Value Types
Payload values are strings that can include:
- **Static text**: Plain text content
- **Dynamic variables**: Context data interpolation using `{variable.name}` syntax
- **HTML content**: Rich formatting and markup
- **URLs**: Links and resource references

## Variable Interpolation

### Syntax
Use curly braces to insert dynamic data: `{variable.path}`

### Available Variables

#### Geographic Data
- `{geo.country}` - User's country
- `{geo.city}` - User's city  
- `{geo.region}` - User's state/region

#### Time Information
- `{time.today}` - Current date
- `{time.hour}` - Current hour (24-format)
- `{time.weekday}` - Day of the week
- `{time.month}` - Current month

#### User Attributes
- `{attribute.user_name}` - Custom user name
- `{attribute.premium_user}` - Premium status
- `{attribute.any_custom_field}` - Any custom attribute passed via API

## Payload Examples

### Simple Static Content
```json
{
  "welcome_message": "Welcome to our service!",
  "cta_button": "Get Started Now",
  "footer_text": "© 2024 Your Company"
}
```

### Geographic Personalization
```json
{
  "location_greeting": "Hello from {geo.city}, {geo.country}!",
  "shipping_info": "Free shipping available in {geo.country}",
  "local_phone": "Call our {geo.city} office for support"
}
```

### Time-Based Content
```json
{
  "time_greeting": "Good morning! Today is {time.weekday}",
  "daily_offer": "Today's special for {time.today}",
  "hour_message": "Current hour: {time.hour}:00"
}
```

### User Personalization
```json
{
  "personal_welcome": "Hi {attribute.user_name}, welcome back!",
  "account_status": "Your {attribute.account_type} membership is active",
  "recommendation": "Based on your preferences, {attribute.user_name}"
}
```

### Rich HTML Content
```json
{
  "banner_html": "<div class='banner'><h1>Welcome {attribute.user_name}!</h1><p>Special offers for {geo.country} customers</p></div>",
  "email_content": "<p>Hello from <strong>{geo.city}</strong>!</p><br><a href='/offers'>View Local Offers</a>"
}
```

### E-commerce Example
```json
{
  "product_banner": "Featured products for {geo.country}",
  "shipping_notice": "Free shipping to {geo.city} on orders over $50",
  "currency_display": "Prices shown in {geo.country} currency",
  "customer_greeting": "Welcome back, {attribute.user_name}!",
  "cart_reminder": "You have {attribute.cart_items} items in your cart"
}
```

### Marketing Campaign
```json
{
  "campaign_title": "Special {time.weekday} Deals!",
  "location_offer": "Exclusive offers for {geo.city} residents",
  "personal_discount": "{attribute.user_name}, your 20% discount expires {time.today}",
  "cta_primary": "Shop Now in {geo.country}",
  "cta_secondary": "Learn More"
}
```

### Mobile App Content
```json
{
  "push_title": "Good {time.hour < 12 ? 'morning' : 'afternoon'}, {attribute.user_name}!",
  "location_feature": "Discover places near {geo.city}",
  "app_greeting": "Welcome to the app, {attribute.user_name}",
  "local_weather": "Weather in {geo.city} today"
}
```

## Variable Processing

### How Variables Work
1. **Request Context**: User sends context data via API
2. **Variable Matching**: System matches `{variable.path}` patterns in payload
3. **Value Replacement**: Replaces variables with actual context data
4. **Response**: Returns processed payload with real values

### Example Processing

**Original Payload:**
```json
{
  "greeting": "Hello {attribute.user_name} from {geo.city}!",
  "offer": "Special discount for {geo.country} customers"
}
```

**User Context:**
```json
{
  "geo": {
    "city": "Berlin",
    "country": "Germany"
  },
  "attribute": {
    "user_name": "Anna"
  }
}
```

**Processed Result:**
```json
{
  "greeting": "Hello Anna from Berlin!",
  "offer": "Special discount for Germany customers"
}
```

## Best Practices

### Content Organization
1. **Descriptive Keys**: Use clear, descriptive key names
2. **Consistent Naming**: Follow naming conventions across variants
3. **Logical Grouping**: Group related content with similar key prefixes
4. **Modular Structure**: Break complex content into smaller, manageable pieces

### Variable Usage
1. **Fallback Values**: Plan for missing variable data
2. **Validation**: Ensure variables match available context data
3. **Testing**: Test with various context scenarios
4. **Documentation**: Document custom attributes and their expected values

### Content Quality
1. **Grammar**: Ensure proper grammar with variable substitution
2. **Formatting**: Test HTML content for proper rendering
3. **Length**: Consider character limits for UI elements
4. **Accessibility**: Include appropriate alt text and labels

## Common Patterns

### Default Fallbacks
```json
{
  "greeting": "Hello {attribute.user_name}!",
  "greeting_fallback": "Hello there!"
}
```

### Conditional Content
```json
{
  "premium_message": "Premium feature available",
  "standard_message": "Upgrade to premium for more features"
}
```

### Multi-language Support
```json
{
  "en_welcome": "Welcome {attribute.user_name}!",
  "de_welcome": "Willkommen {attribute.user_name}!",
  "es_welcome": "¡Bienvenido {attribute.user_name}!"
}
```

### A/B Testing Variants
```json
{
  "version_a_title": "Special Offer - Save 20%!",
  "version_b_title": "Limited Time Deal - 20% Off!"
}
```

## Troubleshooting

### Common Issues
- **Missing Variables**: Variables not replaced indicate missing context data
- **Malformed JSON**: Syntax errors prevent payload processing
- **HTML Escaping**: Ensure proper escaping for HTML content
- **Character Limits**: Long interpolated values may exceed display limits

### Testing Tips
1. **Test with Mock Data**: Use sample context data during development
2. **Edge Cases**: Test with missing or null values
3. **Character Sets**: Verify international characters display correctly
4. **HTML Validation**: Check HTML content renders properly

---

Payloads are the heart of your content delivery. Master variable interpolation and content structure to create dynamic, personalized user experiences. 