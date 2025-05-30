# Variant

## What is a Variant?

A **Variant** is the core feature that defines what will be the actual output of a Content container. Variants contain the real content data (payload) and the rules (conditions) that determine when they should be served to users.

## Key Concepts

- **Content Definition**: Variants contain the actual data that users receive
- **Conditional Logic**: Rules determine when each variant should be served
- **Dynamic Processing**: Payload variables are replaced with real user context data
- **Traffic Control**: Enable/disable variants without deleting them
- **Personalization Engine**: The foundation of all personalized experiences

## Variant Architecture

### Core Components
Every variant consists of four essential elements:

1. **Name**: Human-readable identifier for organization
2. **is_enabled**: Boolean toggle for activation control
3. **Payload**: JSON object containing the actual content data
4. **Conditions**: Array of rules determining when to serve this variant

### How Variants Work
When a user requests content:
1. **Context Analysis**: System evaluates user location, time, and attributes
2. **Condition Matching**: Checks which enabled variants match the user context
3. **Variant Selection**: Selects the appropriate variant based on conditions
4. **Payload Processing**: Replaces variables in payload with actual user data
5. **Content Delivery**: Returns the processed content to the user

## Variant Components

### Payload Structure
The payload contains your actual content as a JSON object:
```json
{
  "welcome_message": "Hello from {geo.city}!",
  "offer_text": "Special deals for {geo.country} customers",
  "personal_greeting": "Hi {attribute.user_name}!"
}
```

### Condition Structure
Conditions determine when the variant should be served:
```json
[
  {
    "logic": "or",
    "rules": [
      {
        "attribute": "geo.city",
        "operator": "in",
        "values": [{"value": "Berlin"}, {"value": "Munich"}]
      }
    ]
  }
]
```

## Common Use Cases

### Geographic Personalization
- **Location-based content**: Different messages for different regions
- **Compliance requirements**: Region-specific legal or regulatory content
- **Language localization**: Content in appropriate languages for each region
- **Cultural adaptation**: Content that respects local customs and preferences

### User Segmentation
- **Experience levels**: Different content for new vs returning users
- **Account types**: Premium vs free user experiences
- **Demographics**: Age, gender, or interest-based personalization
- **Behavior patterns**: Content based on past user actions

### Time-Based Content
- **Business hours**: Different content during operating hours
- **Seasonal campaigns**: Holiday or seasonal promotions
- **Time zone awareness**: Appropriate greetings based on local time
- **Day-of-week targeting**: Weekend vs weekday content

### A/B Testing and Experimentation
- **Message testing**: Compare different promotional messages
- **Design variations**: Test different layouts or visual elements
- **Feature rollouts**: Gradually introduce new features to users
- **Performance optimization**: Find the most effective content approaches

## Variable System

### Available Context Data
Variants can use dynamic variables from:

- **Geographic Data**: `{geo.country}`, `{geo.city}`, `{geo.region}`
- **Time Information**: `{time.hour}`, `{time.weekday}`, `{time.today}`
- **Custom Attributes**: `{attribute.user_name}`, `{attribute.premium_user}`

### Processing Example
**Payload Template:**
```json
{
  "greeting": "Good morning from {geo.city}, {attribute.user_name}!"
}
```

**User Context:**
```json
{
  "geo": {"city": "Berlin"},
  "attribute": {"user_name": "Anna"}
}
```

**Final Output:**
```json
{
  "greeting": "Good morning from Berlin, Anna!"
}
```

## Getting Started

Ready to create your first variant? Follow our comprehensive [Getting Started guide](getting-started.md) to learn how to set up variants with payloads and conditions.

## Advanced Features

### Payload Design
Learn about [Payload](payload.md) structure, variable interpolation, and content organization for effective content delivery.

### Condition Logic
Master [Conditions](conditions.md) to create precise targeting rules that determine when variants should be served to users.

---

Variants are where your actual content lives and where personalization happens. They transform static content into dynamic, context-aware experiences tailored to each user. 