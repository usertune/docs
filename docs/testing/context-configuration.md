# Context Configuration

## Overview

Context configuration is the foundation of effective testing in Usertune. By setting up fake user data, you can simulate different user scenarios and validate how your variants respond to various contexts.

## Context Structure

Test context data is organized into three main categories:

```json
{
  "geo": {
    // Geographic information
  },
  "time": {
    // Time and date information
  },
  "attribute": {
    // Custom user attributes
  }
}
```

## Geographic Data (`geo.xxx`)

Geographic data simulates where your users are located and enables testing of location-based personalization.

### Available Geographic Fields

- **`geo.country`** - User's country
- **`geo.city`** - User's city
- **`geo.region`** - User's state/region/province
- **`geo.latitude`** - Geographic latitude (decimal degrees)
- **`geo.longitude`** - Geographic longitude (decimal degrees)

### Geographic Configuration Examples

#### Basic Location
```json
{
  "geo": {
    "country": "Germany",
    "city": "Berlin"
  }
}
```

#### Detailed Location
```json
{
  "geo": {
    "country": "United States",
    "city": "San Francisco",
    "region": "California",
    "latitude": "37.7749",
    "longitude": "-122.4194"
  }
}
```

#### Unknown Location
```json
{
  "geo": {
    "country": "",
    "city": "",
    "region": ""
  }
}
```

### Common Geographic Test Cases

#### Major Markets
```json
// US Market
{"geo": {"country": "United States", "city": "New York"}}

// European Market  
{"geo": {"country": "Germany", "city": "Berlin"}}

// Asian Market
{"geo": {"country": "Japan", "city": "Tokyo"}}

// UK Market
{"geo": {"country": "United Kingdom", "city": "London"}}
```

#### Regional Testing
```json
// California Users
{
  "geo": {
    "country": "United States",
    "region": "California",
    "city": "Los Angeles"
  }
}

// Texas Users
{
  "geo": {
    "country": "United States", 
    "region": "Texas",
    "city": "Austin"
  }
}
```

## Time Data (`time.xxx`)

Time data simulates when users access your content, enabling testing of time-sensitive personalization.

### Available Time Fields

- **`time.today`** - Current date (YYYY-MM-DD format)
- **`time.hour`** - Current hour (0-23, 24-hour format)
- **`time.weekday`** - Day of the week (Monday, Tuesday, etc.)
- **`time.month`** - Current month (January, February, etc.)
- **`time.year`** - Current year (YYYY format)
- **`time.quarter`** - Current quarter (1, 2, 3, or 4)

### Time Configuration Examples

#### Basic Time
```json
{
  "time": {
    "hour": "14",
    "weekday": "Tuesday"
  }
}
```

#### Complete Time Context
```json
{
  "time": {
    "today": "2024-03-15",
    "hour": "10",
    "weekday": "Friday",
    "month": "March",
    "year": "2024",
    "quarter": "1"
  }
}
```

#### Business Hours
```json
{
  "time": {
    "hour": "9",
    "weekday": "Monday"
  }
}
```

### Common Time Test Cases

#### Business Hours Testing
```json
// Morning business hours
{"time": {"hour": "9", "weekday": "Monday"}}

// Afternoon business hours  
{"time": {"hour": "14", "weekday": "Wednesday"}}

// After hours
{"time": {"hour": "19", "weekday": "Friday"}}
```

#### Weekend Testing
```json
// Saturday afternoon
{"time": {"hour": "15", "weekday": "Saturday"}}

// Sunday morning
{"time": {"hour": "10", "weekday": "Sunday"}}
```

#### Seasonal Testing
```json
// Holiday season
{
  "time": {
    "month": "December",
    "today": "2024-12-15",
    "quarter": "4"
  }
}

// Summer season
{
  "time": {
    "month": "July",
    "today": "2024-07-15", 
    "quarter": "3"
  }
}
```

## Custom Attributes (`attribute.xxx`)

Custom attributes simulate user-specific data and enable testing of user segmentation and personalization.

### Common Attribute Examples

- **`attribute.user_name`** - User's display name
- **`attribute.premium_user`** - Premium subscription status
- **`attribute.age`** - User's age
- **`attribute.language`** - Preferred language
- **`attribute.first_visit`** - Is this their first visit
- **`attribute.cart_items`** - Number of items in shopping cart
- **`attribute.account_type`** - Type of account (free, premium, enterprise)
- **`attribute.registration_date`** - When they registered
- **`attribute.last_login`** - Last login date

### Attribute Configuration Examples

#### Basic User Profile
```json
{
  "attribute": {
    "user_name": "Anna",
    "premium_user": "true",
    "age": "28"
  }
}
```

#### E-commerce User
```json
{
  "attribute": {
    "user_name": "John",
    "account_type": "premium",
    "cart_items": "3",
    "last_purchase": "2024-02-15",
    "loyalty_tier": "gold"
  }
}
```

#### New User
```json
{
  "attribute": {
    "first_visit": "true",
    "premium_user": "false",
    "registration_date": "2024-03-01"
  }
}
```

### Custom Attribute Guidelines

#### Data Types in JSON
All attribute values should be strings in JSON:
```json
{
  "attribute": {
    "age": "25",              // Number as string
    "premium_user": "true",   // Boolean as string
    "user_name": "Sarah"      // String value
  }
}
```

#### Naming Conventions
- Use lowercase with underscores: `user_name`, `premium_user`
- Be descriptive: `registration_date` vs `reg_date`
- Stay consistent across your application

## Complete Context Examples

### Scenario 1: Premium User from Berlin
```json
{
  "geo": {
    "country": "Germany",
    "city": "Berlin",
    "region": "Brandenburg"
  },
  "time": {
    "hour": "10",
    "weekday": "Monday",
    "month": "March"
  },
  "attribute": {
    "user_name": "Anna",
    "premium_user": "true",
    "age": "28",
    "language": "de"
  }
}
```

### Scenario 2: New User from Mobile
```json
{
  "geo": {
    "country": "United States",
    "city": "San Francisco",
    "region": "California"
  },
  "time": {
    "hour": "15",
    "weekday": "Saturday"
  },
  "attribute": {
    "first_visit": "true",
    "device": "mobile",
    "premium_user": "false",
    "age": "22"
  }
}
```

### Scenario 3: Enterprise Customer
```json
{
  "geo": {
    "country": "United Kingdom",
    "city": "London"
  },
  "time": {
    "hour": "11",
    "weekday": "Wednesday",
    "quarter": "2"
  },
  "attribute": {
    "account_type": "enterprise",
    "company_size": "500+",
    "industry": "technology",
    "contact_person": "Sarah Johnson"
  }
}
```

## Context Validation

### Required vs Optional Fields
- **Geographic**: Only include fields you're actually targeting
- **Time**: Include fields your conditions check
- **Attributes**: Match the custom attributes your variants use

### Data Consistency
Ensure your test data makes logical sense:
```json
// Good: Consistent data
{
  "geo": {"country": "Germany", "city": "Berlin"},
  "attribute": {"language": "de"}
}

// Potentially inconsistent
{
  "geo": {"country": "Germany", "city": "Berlin"},
  "attribute": {"language": "en"}
}
```

### Missing Data Testing
Test how your variants handle missing information:
```json
// Minimal context
{
  "attribute": {
    "user_name": "John"
  }
}

// Empty values
{
  "geo": {
    "country": "",
    "city": ""
  },
  "attribute": {
    "premium_user": ""
  }
}
```

## Best Practices

### 1. Start Simple
Begin with basic context and add complexity:
```json
// Start here
{"geo": {"country": "Germany"}}

// Then expand
{
  "geo": {"country": "Germany", "city": "Berlin"},
  "attribute": {"premium_user": "true"}
}
```

### 2. Use Realistic Data
- Real country and city names
- Proper date formats
- Realistic user attributes

### 3. Test Edge Cases
- Empty values
- Missing fields
- Boundary conditions
- Invalid data

### 4. Document Your Context
Keep notes about what each test context represents and why you're testing it.

---

Effective context configuration is key to thorough testing. Build realistic scenarios that match your actual user base and edge cases that might occur in production. 