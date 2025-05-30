# Time Attributes

## Overview

Usertune automatically provides comprehensive time information for every request through the `time.*` attribute namespace. These attributes are calculated server-side based on the current time and enable powerful time-based personalization and scheduling.

## Available Time Attributes

All time attributes are accessible in variant conditions and payloads using the `time.` prefix.

### Date Attributes

#### `time.today`
- **Description**: Current date in YYYY-MM-DD format
- **Format**: ISO 8601 date format (YYYY-MM-DD)
- **Examples**: `"2024-03-15"`, `"2024-12-25"`, `"2024-07-04"`
- **Use Cases**: Date-specific content, daily offers, event scheduling

```json
{
  "attribute": "time.today",
  "operator": "equals",
  "values": [{"value": "2024-12-25"}]
}
```

#### `time.yesterday`
- **Description**: Yesterday's date in YYYY-MM-DD format
- **Format**: ISO 8601 date format (YYYY-MM-DD)
- **Examples**: `"2024-03-14"`, `"2024-12-24"`, `"2024-07-03"`
- **Use Cases**: "Yesterday's news" content, retrospective messaging

```json
{
  "message": "Did you miss yesterday's ({time.yesterday}) special offer?"
}
```

#### `time.tomorrow`
- **Description**: Tomorrow's date in YYYY-MM-DD format
- **Format**: ISO 8601 date format (YYYY-MM-DD)
- **Examples**: `"2024-03-16"`, `"2024-12-26"`, `"2024-07-05"`
- **Use Cases**: Future event promotion, "coming tomorrow" messages

```json
{
  "preview": "Tomorrow ({time.tomorrow}) we're launching something special!"
}
```

### Time Components

#### `time.year`
- **Description**: Current year
- **Format**: Four-digit year (YYYY)
- **Examples**: `"2024"`, `"2025"`, `"2023"`
- **Use Cases**: Copyright notices, annual campaigns, year-based content

```json
{
  "copyright": "© {time.year} Your Company Name"
}
```

#### `time.month`
- **Description**: Current month number
- **Format**: Two-digit month (01-12)
- **Examples**: `"01"` (January), `"12"` (December), `"07"` (July)
- **Use Cases**: Monthly promotions, seasonal content, billing cycles

```json
{
  "attribute": "time.month",
  "operator": "in",
  "values": [{"value": "12"}, {"value": "01"}, {"value": "02"}]
}
```

#### `time.day`
- **Description**: Current day of the month
- **Format**: Two-digit day (01-31)
- **Examples**: `"01"`, `"15"`, `"31"`
- **Use Cases**: Monthly events, payday targeting, end-of-month campaigns

```json
{
  "attribute": "time.day",
  "operator": "equals",
  "values": [{"value": "01"}]
}
```

#### `time.hour`
- **Description**: Current hour in 24-hour format
- **Format**: Two-digit hour (00-23)
- **Examples**: `"00"` (midnight), `"12"` (noon), `"23"` (11 PM)
- **Use Cases**: Business hours, time-of-day messaging, shift-based content

```json
{
  "attribute": "time.hour",
  "operator": "greater_than_or_equal",
  "values": [{"value": "09"}]
}
```

#### `time.minute`
- **Description**: Current minute
- **Format**: Two-digit minute (00-59)
- **Examples**: `"00"`, `"30"`, `"59"`
- **Use Cases**: Precise timing, minute-based campaigns, real-time updates

```json
{
  "real_time": "Current time: {time.hour}:{time.minute}"
}
```

#### `time.second`
- **Description**: Current second
- **Format**: Two-digit second (00-59)
- **Examples**: `"00"`, `"30"`, `"59"`
- **Use Cases**: Real-time displays, precise timing, countdowns

```json
{
  "timestamp": "Last updated: {time.hour}:{time.minute}:{time.second}"
}
```

### Calendar Attributes

#### `time.weekday`
- **Description**: Current day of the week
- **Format**: Full weekday name in English
- **Examples**: `"Monday"`, `"Tuesday"`, `"Wednesday"`, `"Thursday"`, `"Friday"`, `"Saturday"`, `"Sunday"`
- **Use Cases**: Weekday vs weekend content, day-specific promotions

```json
{
  "attribute": "time.weekday",
  "operator": "in",
  "values": [{"value": "Saturday"}, {"value": "Sunday"}]
}
```

#### `time.quarter`
- **Description**: Current fiscal quarter
- **Format**: Quarter notation (Q1, Q2, Q3, Q4)
- **Examples**: `"Q1"` (Jan-Mar), `"Q2"` (Apr-Jun), `"Q3"` (Jul-Sep), `"Q4"` (Oct-Dec)
- **Use Cases**: Quarterly reports, seasonal campaigns, business cycles

```json
{
  "attribute": "time.quarter",
  "operator": "equals",
  "values": [{"value": "Q4"}]
}
```

#### `time.week`
- **Description**: ISO week number of the year
- **Format**: Week number (1-53)
- **Examples**: `"1"`, `"26"`, `"53"`
- **Use Cases**: Weekly campaigns, week-based scheduling, analytics periods

```json
{
  "attribute": "time.week",
  "operator": "greater_than",
  "values": [{"value": "26"}]
}
```

#### `time.day_of_year`
- **Description**: Day number within the current year
- **Format**: Day number (1-366)
- **Examples**: `"1"` (Jan 1), `"365"` (Dec 31), `"100"` (April 10)
- **Use Cases**: Year progress tracking, special date calculations

```json
{
  "progress": "Day {time.day_of_year} of {time.year}"
}
```

### Technical Time Formats

#### `time.timestamp`
- **Description**: Unix timestamp (seconds since epoch)
- **Format**: Unix timestamp as string
- **Examples**: `"1710509400"`, `"1703527200"`
- **Use Cases**: Technical integrations, precise time calculations, caching

```json
{
  "cache_key": "content_{time.timestamp}"
}
```

#### `time.iso8601`
- **Description**: ISO 8601 formatted timestamp with timezone
- **Format**: RFC3339/ISO 8601 format
- **Examples**: `"2024-03-15T14:30:00Z"`, `"2024-12-25T00:00:00Z"`
- **Use Cases**: API integrations, international time coordination, logging

```json
{
  "created_at": "{time.iso8601}"
}
```

## Usage Examples

### Business Hours Targeting

#### Office Hours Only
```json
// Condition for business hours (9 AM - 5 PM)
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.hour",
        "operator": "greater_than_or_equal",
        "values": [{"value": "09"}]
      },
      {
        "attribute": "time.hour",
        "operator": "less_than",
        "values": [{"value": "17"}]
      },
      {
        "attribute": "time.weekday",
        "operator": "not_in",
        "values": [{"value": "Saturday"}, {"value": "Sunday"}]
      }
    ]
  }
]

// Business hours payload
{
  "message": "Our office is open! Call us now.",
  "phone": "+1-555-0123",
  "chat_available": "true"
}
```

#### After Hours Support
```json
// Condition for after hours
[
  {
    "logic": "or",
    "rules": [
      {
        "attribute": "time.hour",
        "operator": "less_than",
        "values": [{"value": "09"}]
      },
      {
        "attribute": "time.hour",
        "operator": "greater_than_or_equal",
        "values": [{"value": "17"}]
      },
      {
        "attribute": "time.weekday",
        "operator": "in",
        "values": [{"value": "Saturday"}, {"value": "Sunday"}]
      }
    ]
  }
]

// After hours payload
{
  "message": "We're currently closed. Leave a message!",
  "support_form": "/contact",
  "chat_available": "false"
}
```

### Seasonal and Holiday Campaigns

#### Holiday Season (Q4)
```json
// Condition for Q4 (October-December)
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.quarter",
        "operator": "equals",
        "values": [{"value": "Q4"}]
      }
    ]
  }
]

// Holiday season payload
{
  "banner": "🎄 Holiday Special Offers!",
  "discount": "25% off everything",
  "theme": "holiday",
  "gift_wrapping": "available"
}
```

#### Weekend Promotions
```json
// Weekend condition
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.weekday",
        "operator": "in",
        "values": [{"value": "Saturday"}, {"value": "Sunday"}]
      }
    ]
  }
]

// Weekend payload
{
  "title": "Weekend Warriors Sale!",
  "discount": "Weekend special: 20% off",
  "valid_until": "Sunday midnight"
}
```

### Daily and Time-Based Content

#### Morning Greetings
```json
// Morning hours (6 AM - 12 PM)
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.hour",
        "operator": "greater_than_or_equal",
        "values": [{"value": "06"}]
      },
      {
        "attribute": "time.hour",
        "operator": "less_than",
        "values": [{"value": "12"}]
      }
    ]
  }
]

// Morning payload
{
  "greeting": "Good morning! ☀️",
  "coffee_offer": "Start your day with 15% off coffee",
  "breakfast_menu": "/breakfast"
}
```

#### Evening Content
```json
// Evening hours (6 PM - 11 PM)
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.hour",
        "operator": "greater_than_or_equal",
        "values": [{"value": "18"}]
      },
      {
        "attribute": "time.hour",
        "operator": "less_than",
        "values": [{"value": "23"}]
      }
    ]
  }
]

// Evening payload
{
  "greeting": "Good evening! 🌙",
  "dinner_special": "Evening dining: 20% off dinner",
  "night_mode": "enabled"
}
```

### Monthly and Quarterly Campaigns

#### End of Month Push
```json
// Last 3 days of month
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.day",
        "operator": "greater_than_or_equal",
        "values": [{"value": "28"}]
      }
    ]
  }
]

// End of month payload
{
  "urgency": "Month-end clearance!",
  "message": "Last chance for monthly deals",
  "countdown": "Ends in {32 - time.day} days"
}
```

#### New Year Campaign
```json
// January 1st
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.month",
        "operator": "equals",
        "values": [{"value": "01"}]
      },
      {
        "attribute": "time.day",
        "operator": "equals",
        "values": [{"value": "01"}]
      }
    ]
  }
]

// New Year payload
{
  "celebration": "🎉 Happy New Year {time.year}!",
  "resolution": "New year, new savings!",
  "fresh_start": "true"
}
```

## Dynamic Time Content

### Real-Time Information
```json
{
  "current_time": "Today is {time.weekday}, {time.today}",
  "quarter_info": "We're in {time.quarter} of {time.year}",
  "week_progress": "Week {time.week} of {time.year}",
  "daily_progress": "Day {time.day_of_year} of the year"
}
```

### Time-Aware Messaging
```json
{
  "greeting": "Good {time.hour < 12 ? 'morning' : time.hour < 18 ? 'afternoon' : 'evening'}!",
  "day_context": "Happy {time.weekday}!",
  "month_context": "Welcome to month {time.month} of {time.year}",
  "timestamp": "Last updated: {time.hour}:{time.minute}"
}
```

## Advanced Time-Based Targeting

### Multi-Condition Time Targeting
```json
// Weekday lunch hours
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.weekday",
        "operator": "not_in",
        "values": [{"value": "Saturday"}, {"value": "Sunday"}]
      },
      {
        "attribute": "time.hour",
        "operator": "greater_than_or_equal",
        "values": [{"value": "11"}]
      },
      {
        "attribute": "time.hour",
        "operator": "less_than_or_equal",
        "values": [{"value": "14"}]
      }
    ]
  }
]
```

### Seasonal Transitions
```json
// Spring season (March-May)
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.month",
        "operator": "in",
        "values": [{"value": "03"}, {"value": "04"}, {"value": "05"}]
      }
    ]
  }
]
```

## Best Practices

### 1. Time Zone Considerations
- Time attributes use server time (UTC by default)
- Consider user location for time-sensitive content
- Combine with `geo.time_zone` for local time awareness

### 2. Business Logic
- Account for different business hours across regions
- Consider cultural differences in weekend days
- Plan for leap years and month variations

### 3. Performance Optimization
- Cache time-based content appropriately
- Use broader time ranges to improve cache hit rates
- Consider using `time.hour` instead of `time.minute` for better caching

### 4. User Experience
- Provide clear time context in messaging
- Use friendly time formats in user-facing content
- Consider internationalization for time displays

## Testing Time-Based Variants

### Testing Different Times
```json
// Test morning content
{
  "time": {
    "hour": "08",
    "weekday": "Monday",
    "today": "2024-03-15"
  }
}

// Test evening content
{
  "time": {
    "hour": "19",
    "weekday": "Friday",
    "today": "2024-03-15"
  }
}

// Test weekend content
{
  "time": {
    "hour": "14",
    "weekday": "Saturday",
    "today": "2024-03-16"
  }
}
```

### Testing Seasonal Content
```json
// Test holiday season
{
  "time": {
    "month": "12",
    "quarter": "Q4",
    "day": "25",
    "year": "2024"
  }
}

// Test summer content
{
  "time": {
    "month": "07",
    "quarter": "Q3",
    "weekday": "Wednesday"
  }
}
```

## Limitations and Notes

### Server Time Dependency
- All time attributes are based on server time
- Server time zone affects time calculations
- Consider UTC for global applications

### Caching Implications
- Time-based content has shorter cache lifespans
- Minute-level precision reduces cache efficiency
- Balance precision with performance needs

### Update Frequency
- Time attributes update on each request
- High-frequency time changes require careful caching strategy
- Consider using broader time units for better performance

---

Time attributes enable sophisticated temporal targeting and time-aware personalization. Use them to create content that responds to time of day, seasons, business hours, and calendar events to deliver timely and relevant user experiences. 