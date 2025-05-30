# Getting Started with Variants

Variants are the core feature that defines what will be the actual output of a Content container. Each variant contains the real content data and rules for when it should be served to users.

## What is a Variant?

A **Variant** is a specific version of content that includes:
- **Content Data**: The actual information that users will receive
- **Conditions**: Rules that determine when this variant should be served
- **Control Settings**: Configuration for enabling/disabling the variant

Variants are the building blocks that Content containers use to deliver personalized experiences.

## Core Variant Components

Every variant has four essential components:

### 1. Name
- **Purpose**: Human-readable identifier for the variant
- **Example**: "Berlin Welcome Message", "US Holiday Promotion", "New User Onboarding"
- **Usage**: Helps organize and identify variants within a content container

### 2. is_enabled
- **Purpose**: Boolean toggle to control variant availability
- **Values**: `true` (active) or `false` (inactive)
- **Usage**: Temporarily disable variants without deleting them

### 3. Payload
- **Purpose**: The actual content data that users receive
- **Format**: JSON object with key-value pairs
- **Example**: 
  ```json
  {
    "text_banner": "Hello from city {geo.city}!",
    "text_welcome": "Welcome from country {geo.country}!",
    "text_person_welcome": "Hi {attribute.user_name}!"
  }
  ```

### 4. Conditions
- **Purpose**: Rules that determine when this variant should be served
- **Format**: Array of logical conditions with rules
- **Example**:
  ```json
  [
    {
      "logic": "or",
      "rules": [
        {
          "attribute": "geo.city",
          "operator": "in",
          "values": [{"value": "Berlin"}]
        },
        {
          "attribute": "geo.city", 
          "operator": "contains",
          "values": [{"value": "New"}]
        }
      ]
    },
    {
      "logic": "or",
      "rules": [
        {
          "attribute": "geo.country",
          "operator": "equals", 
          "values": [{"value": "Germany"}]
        }
      ]
    }
  ]
  ```

## How Variants Work

### Content Selection Process
When a user requests content:

1. **Content Container**: Receives the API request with user context
2. **Variant Evaluation**: Checks each enabled variant's conditions against user data
3. **Condition Matching**: Finds variants whose conditions match the user context
4. **Variant Selection**: Selects the appropriate variant based on priority/rules
5. **Payload Processing**: Processes the variant's payload with user context data
6. **Response**: Returns the processed content to the user

### Example Scenario

**User Context:**
```json
{
  "geo": {
    "city": "Berlin",
    "country": "Germany"
  },
  "attributes": {
    "user_name": "Anna"
  }
}
```

**Variant Conditions:** City is "Berlin" OR contains "New" OR country is "Germany"

**Result:** Conditions match! Variant is selected.

**Payload Processing:**
```json
{
  "text_banner": "Hello from city Berlin!",
  "text_welcome": "Welcome from country Germany!",
  "text_person_welcome": "Hi Anna!"
}
```

## Prerequisites

Before creating variants, you need:
- A Usertune account
- A workspace created
- A content container created (see [Content Getting Started](../content/getting-started.md))

## Step 1: Access Your Content Container

1. Sign in to [app.usertune.io](https://app.usertune.io)
2. Navigate to your workspace
3. Click on the content container where you want to add variants
4. You'll see the content dashboard with existing variants (if any)

## Step 2: Create New Variant

1. Click **"Add Variant"** or **"New Variant"** button
2. A variant creation form will appear

### Basic Configuration:
- **Name**: Enter a descriptive name
  - Example: "Berlin City Welcome", "Default Message", "Holiday Promotion"
- **is_enabled**: Toggle to enable the variant (default: enabled)

## Step 3: Configure Payload

Define what content this variant will deliver:

### Simple Payload Example:
```json
{
  "message": "Welcome to our service!",
  "cta_text": "Get Started"
}
```

### Dynamic Payload Example:
```json
{
  "greeting": "Hello from {geo.city}!",
  "offer": "Special discount for {geo.country} customers",
  "personalized": "Hi {attribute.user_name}, welcome back!"
}
```

## Step 4: Set Conditions

Define when this variant should be served:

### Geographic Condition:
```json
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.country",
        "operator": "equals",
        "values": [{"value": "Germany"}]
      }
    ]
  }
]
```

### Multiple Conditions:
```json
[
  {
    "logic": "or", 
    "rules": [
      {
        "attribute": "geo.city",
        "operator": "in",
        "values": [
          {"value": "Berlin"},
          {"value": "Munich"}
        ]
      }
    ]
  },
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.premium_user",
        "operator": "equals",
        "values": [{"value": "true"}]
      }
    ]
  }
]
```

## Step 5: Test and Save

1. **Test Conditions**: Verify your conditions work with sample user data
2. **Test Payload**: Check that payload processing works correctly
3. **Save Variant**: Click "Save" or "Create" to finalize

## Common Variant Patterns

### Default Variant
- **Name**: "Default"
- **Conditions**: No conditions (catches all users who don't match other variants)
- **Purpose**: Fallback content when no other variants match

### Geographic Variants
- **Name**: "US Users", "EU Users", "APAC Users"
- **Conditions**: Based on country or region
- **Purpose**: Location-specific content and compliance

### User Segment Variants
- **Name**: "New Users", "Premium Users", "Returning Customers"
- **Conditions**: Based on user attributes
- **Purpose**: Personalized experiences for different user types

### A/B Testing Variants
- **Name**: "Version A", "Version B"
- **Conditions**: Random distribution or user segments
- **Purpose**: Compare different content approaches

## Next Steps

After creating your variants:

1. **Learn Payload Details**: Explore [Payload](payload.md) for advanced content configuration
2. **Master Conditions**: Study [Conditions](conditions.md) for complex targeting rules
3. **Test Thoroughly**: Verify variants work with different user contexts
4. **Monitor Performance**: Track which variants perform best

---

Variants are where your actual content lives and where the magic of personalization happens. Master payloads and conditions to create powerful, targeted user experiences. 