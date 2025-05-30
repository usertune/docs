# Test Scenarios

## Overview

This page provides comprehensive test scenarios that you can use as templates for validating your Content and Variants. Each scenario includes complete context configuration and explains what it's designed to test.

## Geographic Targeting Scenarios

### Scenario 1: Major Market Testing
Test how your variants respond to users from different major markets.

#### US Market User
```json
{
  "geo": {
    "country": "United States",
    "city": "New York",
    "region": "New York"
  },
  "time": {
    "hour": "14",
    "weekday": "Tuesday"
  },
  "attribute": {
    "user_name": "Sarah",
    "language": "en"
  }
}
```

#### European Market User
```json
{
  "geo": {
    "country": "Germany",
    "city": "Berlin",
    "region": "Brandenburg"
  },
  "time": {
    "hour": "14",
    "weekday": "Tuesday"
  },
  "attribute": {
    "user_name": "Anna",
    "language": "de"
  }
}
```

#### Asian Market User
```json
{
  "geo": {
    "country": "Japan",
    "city": "Tokyo"
  },
  "time": {
    "hour": "14",
    "weekday": "Tuesday"
  },
  "attribute": {
    "user_name": "Yuki",
    "language": "ja"
  }
}
```

### Scenario 2: Regional Targeting
Test variants designed for specific regions within countries.

#### California User
```json
{
  "geo": {
    "country": "United States",
    "city": "San Francisco",
    "region": "California"
  },
  "attribute": {
    "user_name": "Mike",
    "time_zone": "PST"
  }
}
```

#### Texas User
```json
{
  "geo": {
    "country": "United States",
    "city": "Austin",
    "region": "Texas"
  },
  "attribute": {
    "user_name": "John",
    "time_zone": "CST"
  }
}
```

### Scenario 3: Unknown Location
Test fallback behavior when location data is missing or incomplete.

#### Completely Unknown Location
```json
{
  "geo": {
    "country": "",
    "city": "",
    "region": ""
  },
  "attribute": {
    "user_name": "Anonymous"
  }
}
```

#### Partial Location Data
```json
{
  "geo": {
    "country": "United States",
    "city": "",
    "region": ""
  },
  "attribute": {
    "user_name": "PartialUser"
  }
}
```

## Time-Based Scenarios

### Scenario 4: Business Hours Testing
Test variants designed for different times of day.

#### Morning Business Hours
```json
{
  "time": {
    "hour": "9",
    "weekday": "Monday",
    "today": "2024-03-18"
  },
  "geo": {
    "country": "United States",
    "city": "Chicago"
  },
  "attribute": {
    "user_name": "Alex"
  }
}
```

#### Afternoon Peak Hours
```json
{
  "time": {
    "hour": "14",
    "weekday": "Wednesday",
    "today": "2024-03-20"
  },
  "geo": {
    "country": "United States",
    "city": "Denver"
  },
  "attribute": {
    "user_name": "Jordan"
  }
}
```

#### After Hours
```json
{
  "time": {
    "hour": "19",
    "weekday": "Friday",
    "today": "2024-03-22"
  },
  "geo": {
    "country": "United States",
    "city": "Seattle"
  },
  "attribute": {
    "user_name": "Casey"
  }
}
```

### Scenario 5: Weekend vs Weekday
Test different content for weekend and weekday users.

#### Weekend User
```json
{
  "time": {
    "hour": "11",
    "weekday": "Saturday",
    "today": "2024-03-23"
  },
  "geo": {
    "country": "Canada",
    "city": "Toronto"
  },
  "attribute": {
    "user_name": "Sam",
    "weekend_shopper": "true"
  }
}
```

#### Weekday User
```json
{
  "time": {
    "hour": "12",
    "weekday": "Tuesday",
    "today": "2024-03-19"
  },
  "geo": {
    "country": "Canada",
    "city": "Vancouver"
  },
  "attribute": {
    "user_name": "Taylor",
    "work_break": "true"
  }
}
```

### Scenario 6: Seasonal Content
Test seasonal and holiday-specific variants.

#### Holiday Season
```json
{
  "time": {
    "today": "2024-12-15",
    "month": "December",
    "quarter": "4",
    "hour": "16",
    "weekday": "Sunday"
  },
  "geo": {
    "country": "United States",
    "city": "Boston"
  },
  "attribute": {
    "user_name": "Holiday Shopper",
    "shopping_for_gifts": "true"
  }
}
```

#### Summer Campaign
```json
{
  "time": {
    "today": "2024-07-15",
    "month": "July",
    "quarter": "3",
    "hour": "14",
    "weekday": "Monday"
  },
  "geo": {
    "country": "Australia",
    "city": "Sydney"
  },
  "attribute": {
    "user_name": "Summer User",
    "season": "summer"
  }
}
```

## User Segmentation Scenarios

### Scenario 7: Account Type Testing
Test variants for different types of user accounts.

#### Premium User
```json
{
  "geo": {
    "country": "United Kingdom",
    "city": "London"
  },
  "time": {
    "hour": "10",
    "weekday": "Thursday"
  },
  "attribute": {
    "user_name": "Emma",
    "account_type": "premium",
    "premium_user": "true",
    "subscription_tier": "gold",
    "member_since": "2022-01-15"
  }
}
```

#### Free User
```json
{
  "geo": {
    "country": "United Kingdom",
    "city": "Manchester"
  },
  "time": {
    "hour": "15",
    "weekday": "Thursday"
  },
  "attribute": {
    "user_name": "James",
    "account_type": "free",
    "premium_user": "false",
    "trial_days_left": "7"
  }
}
```

#### Enterprise User
```json
{
  "geo": {
    "country": "United States",
    "city": "San Francisco"
  },
  "time": {
    "hour": "11",
    "weekday": "Wednesday"
  },
  "attribute": {
    "user_name": "Enterprise Admin",
    "account_type": "enterprise",
    "company_name": "TechCorp Inc",
    "company_size": "1000+",
    "admin_user": "true"
  }
}
```

### Scenario 8: User Journey Testing
Test variants for different stages of the user journey.

#### First-Time Visitor
```json
{
  "geo": {
    "country": "France",
    "city": "Paris"
  },
  "time": {
    "hour": "13",
    "weekday": "Monday"
  },
  "attribute": {
    "first_visit": "true",
    "session_count": "1",
    "referral_source": "google",
    "premium_user": "false"
  }
}
```

#### Returning User
```json
{
  "geo": {
    "country": "France",
    "city": "Lyon"
  },
  "time": {
    "hour": "16",
    "weekday": "Friday"
  },
  "attribute": {
    "user_name": "Marie",
    "first_visit": "false",
    "session_count": "15",
    "last_visit": "2024-03-15",
    "premium_user": "true"
  }
}
```

#### Churned User Returning
```json
{
  "geo": {
    "country": "Spain",
    "city": "Madrid"
  },
  "time": {
    "hour": "18",
    "weekday": "Tuesday"
  },
  "attribute": {
    "user_name": "Carlos",
    "first_visit": "false",
    "last_visit": "2023-12-01",
    "churned_user": "true",
    "premium_user": "false",
    "reactivation_campaign": "true"
  }
}
```

## E-commerce Scenarios

### Scenario 9: Shopping Behavior
Test variants based on shopping patterns and cart status.

#### User with Items in Cart
```json
{
  "geo": {
    "country": "Netherlands",
    "city": "Amsterdam"
  },
  "time": {
    "hour": "20",
    "weekday": "Sunday"
  },
  "attribute": {
    "user_name": "Lisa",
    "cart_items": "3",
    "cart_value": "150.00",
    "abandoned_cart": "false",
    "premium_user": "true"
  }
}
```

#### Abandoned Cart User
```json
{
  "geo": {
    "country": "Italy",
    "city": "Rome"
  },
  "time": {
    "hour": "10",
    "weekday": "Monday"
  },
  "attribute": {
    "user_name": "Marco",
    "cart_items": "2",
    "cart_value": "89.99",
    "abandoned_cart": "true",
    "hours_since_abandonment": "24"
  }
}
```

#### Frequent Buyer
```json
{
  "geo": {
    "country": "Germany",
    "city": "Munich"
  },
  "time": {
    "hour": "12",
    "weekday": "Saturday"
  },
  "attribute": {
    "user_name": "Klaus",
    "total_orders": "25",
    "last_order": "2024-03-10",
    "average_order_value": "125.00",
    "loyalty_tier": "platinum"
  }
}
```

### Scenario 10: Promotional Testing
Test promotional variants for different customer segments.

#### First Purchase Incentive
```json
{
  "geo": {
    "country": "Sweden",
    "city": "Stockholm"
  },
  "time": {
    "hour": "14",
    "weekday": "Wednesday"
  },
  "attribute": {
    "user_name": "Erik",
    "total_orders": "0",
    "cart_items": "1",
    "first_purchase_discount": "available",
    "registration_date": "2024-03-15"
  }
}
```

#### Loyalty Reward
```json
{
  "geo": {
    "country": "Norway",
    "city": "Oslo"
  },
  "time": {
    "hour": "17",
    "weekday": "Friday"
  },
  "attribute": {
    "user_name": "Ingrid",
    "loyalty_points": "2500",
    "loyalty_tier": "gold",
    "points_expiring_soon": "true",
    "last_redemption": "2024-01-15"
  }
}
```

## Mobile and Device Scenarios

### Scenario 11: Device-Specific Testing
Test variants designed for different devices and platforms.

#### Mobile User
```json
{
  "geo": {
    "country": "Brazil",
    "city": "São Paulo"
  },
  "time": {
    "hour": "8",
    "weekday": "Monday"
  },
  "attribute": {
    "user_name": "Ana",
    "device": "mobile",
    "platform": "ios",
    "app_version": "2.1.0",
    "push_notifications": "enabled"
  }
}
```

#### Desktop User
```json
{
  "geo": {
    "country": "Brazil",
    "city": "Rio de Janeiro"
  },
  "time": {
    "hour": "14",
    "weekday": "Tuesday"
  },
  "attribute": {
    "user_name": "Pedro",
    "device": "desktop",
    "browser": "chrome",
    "screen_size": "large",
    "ad_blocker": "false"
  }
}
```

## A/B Testing Scenarios

### Scenario 12: Control vs Variant Testing
Test different versions of content for A/B testing.

#### Control Group User
```json
{
  "geo": {
    "country": "Australia",
    "city": "Melbourne"
  },
  "time": {
    "hour": "11",
    "weekday": "Thursday"
  },
  "attribute": {
    "user_name": "Chris",
    "ab_test_group": "control",
    "experiment_id": "homepage_cta_test",
    "user_id": "12345"
  }
}
```

#### Test Group User
```json
{
  "geo": {
    "country": "Australia",
    "city": "Sydney"
  },
  "time": {
    "hour": "11",
    "weekday": "Thursday"
  },
  "attribute": {
    "user_name": "Alex",
    "ab_test_group": "variant_a",
    "experiment_id": "homepage_cta_test",
    "user_id": "67890"
  }
}
```

## Edge Case Scenarios

### Scenario 13: Minimal Data
Test how variants handle minimal context information.

#### Bare Minimum Context
```json
{
  "attribute": {
    "user_name": "MinimalUser"
  }
}
```

#### Only Geographic Data
```json
{
  "geo": {
    "country": "Canada"
  }
}
```

### Scenario 14: Invalid or Unusual Data
Test resilience with unexpected data.

#### Invalid Time Values
```json
{
  "time": {
    "hour": "25",
    "weekday": "InvalidDay"
  },
  "geo": {
    "country": "Test Country"
  },
  "attribute": {
    "user_name": "EdgeCase"
  }
}
```

#### Empty String Values
```json
{
  "geo": {
    "country": "",
    "city": ""
  },
  "time": {
    "hour": "",
    "weekday": ""
  },
  "attribute": {
    "user_name": "",
    "premium_user": ""
  }
}
```

## Testing Workflow with Scenarios

### 1. Choose Relevant Scenarios
Select scenarios that match your actual user base and business logic.

### 2. Customize for Your Use Case
Modify the example attributes to match your specific requirements.

### 3. Test Systematically
- Start with basic scenarios
- Progress to complex user journeys
- Finish with edge cases

### 4. Document Results
Keep track of which scenarios work as expected and which need adjustment.

---

These scenarios provide a comprehensive foundation for testing your personalization logic. Adapt them to match your specific use cases and always test both happy paths and edge cases to ensure robust content delivery. 