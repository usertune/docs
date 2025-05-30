# Variant Conditions

## Overview

**Conditions** determine when a variant should be served to users. They are JSON arrays containing logical rules that evaluate user context data to decide if a variant matches the current request.

## Condition Structure

### Basic Format
Conditions are arrays of condition groups:

```json
[
  {
    "logic": "and|or",
    "rules": [
      {
        "attribute": "context.path",
        "operator": "equals|contains|in|...",
        "values": [{"value": "target_value"}]
      }
    ]
  }
]
```

### Condition Components

#### Logic Types
- **"and"**: All rules in the group must match
- **"or"**: At least one rule in the group must match

#### Rule Structure
Each rule contains:
- **attribute**: The context path to evaluate
- **operator**: How to compare the attribute value
- **values**: Array of target values to compare against

## Available Operators

### Equality Operators

#### equals
Exact string match (case-sensitive):
```json
{
  "attribute": "geo.country",
  "operator": "equals",
  "values": [{"value": "Germany"}]
}
```

#### not_equals
Does not match exactly:
```json
{
  "attribute": "geo.country", 
  "operator": "not_equals",
  "values": [{"value": "United States"}]
}
```

### Text Operators

#### contains
String contains substring (case-sensitive):
```json
{
  "attribute": "geo.city",
  "operator": "contains", 
  "values": [{"value": "New"}]
}
```
*Matches: "New York", "New Delhi", "Newcastle"*

#### not_contains
String does not contain substring:
```json
{
  "attribute": "geo.city",
  "operator": "not_contains",
  "values": [{"value": "San"}]
}
```

#### starts_with
String begins with specified text:
```json
{
  "attribute": "geo.city",
  "operator": "starts_with",
  "values": [{"value": "San"}]
}
```
*Matches: "San Francisco", "San Diego", "Santa Barbara"*

#### ends_with
String ends with specified text:
```json
{
  "attribute": "geo.city", 
  "operator": "ends_with",
  "values": [{"value": "ton"}]
}
```
*Matches: "Washington", "Boston", "Houston"*

### List Operators

#### in
Value is in the specified list:
```json
{
  "attribute": "geo.country",
  "operator": "in",
  "values": [
    {"value": "Germany"},
    {"value": "France"}, 
    {"value": "Italy"}
  ]
}
```

#### not_in
Value is not in the specified list:
```json
{
  "attribute": "geo.country",
  "operator": "not_in",
  "values": [
    {"value": "Russia"},
    {"value": "China"}
  ]
}
```

### Numeric Operators

#### greater_than
Numeric value is greater than target:
```json
{
  "attribute": "time.hour",
  "operator": "greater_than",
  "values": [{"value": "12"}]
}
```

#### less_than
Numeric value is less than target:
```json
{
  "attribute": "time.hour",
  "operator": "less_than", 
  "values": [{"value": "18"}]
}
```

#### greater_than_or_equal
Numeric value is greater than or equal to target:
```json
{
  "attribute": "attribute.age",
  "operator": "greater_than_or_equal",
  "values": [{"value": "18"}]
}
```

#### less_than_or_equal
Numeric value is less than or equal to target:
```json
{
  "attribute": "attribute.age",
  "operator": "less_than_or_equal", 
  "values": [{"value": "65"}]
}
```

### Boolean Operators

#### is_true
Boolean value is true:
```json
{
  "attribute": "attribute.premium_user",
  "operator": "is_true",
  "values": []
}
```

#### is_false
Boolean value is false:
```json
{
  "attribute": "attribute.first_visit",
  "operator": "is_false", 
  "values": []
}
```

### Existence Operators

#### exists
Attribute has any value (not null/undefined):
```json
{
  "attribute": "attribute.user_name",
  "operator": "exists",
  "values": []
}
```

#### not_exists
Attribute is null, undefined, or missing:
```json
{
  "attribute": "attribute.phone_number",
  "operator": "not_exists",
  "values": []
}
```

## Available Attributes

### Geographic Context
- `geo.country` - User's country
- `geo.city` - User's city
- `geo.region` - User's state/region
- `geo.latitude` - Geographic latitude
- `geo.longitude` - Geographic longitude

### Time Context
- `time.today` - Current date
- `time.hour` - Current hour (0-23)
- `time.weekday` - Day of week (Monday, Tuesday, etc.)
- `time.month` - Current month
- `time.year` - Current year
- `time.quarter` - Current quarter (1-4)

### Custom Attributes
- `attribute.any_field` - Any custom attribute passed via API
- Common examples: `attribute.user_name`, `attribute.premium_user`, `attribute.age`

## Condition Examples

### Simple Geographic Targeting
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

### Multiple Cities (OR Logic)
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
          {"value": "Munich"},
          {"value": "Hamburg"}
        ]
      }
    ]
  }
]
```

### Time-Based Conditions
```json
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "time.hour",
        "operator": "greater_than_or_equal", 
        "values": [{"value": "9"}]
      },
      {
        "attribute": "time.hour",
        "operator": "less_than",
        "values": [{"value": "17"}]
      }
    ]
  }
]
```

### User Segmentation
```json
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.premium_user",
        "operator": "is_true",
        "values": []
      },
      {
        "attribute": "attribute.age",
        "operator": "greater_than_or_equal",
        "values": [{"value": "25"}]
      }
    ]
  }
]
```

### Complex Multi-Group Conditions
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
      },
      {
        "attribute": "geo.city",
        "operator": "contains",
        "values": [{"value": "New"}]
      }
    ]
  },
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "attribute.premium_user",
        "operator": "is_true", 
        "values": []
      }
    ]
  }
]
```

### Weekend Promotion
```json
[
  {
    "logic": "or",
    "rules": [
      {
        "attribute": "time.weekday",
        "operator": "in",
        "values": [
          {"value": "Saturday"},
          {"value": "Sunday"}
        ]
      }
    ]
  }
]
```

### Regional Campaign
```json
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.country",
        "operator": "in",
        "values": [
          {"value": "Germany"},
          {"value": "Austria"},
          {"value": "Switzerland"}
        ]
      },
      {
        "attribute": "attribute.language",
        "operator": "equals",
        "values": [{"value": "de"}]
      }
    ]
  }
]
```

## Logic Evaluation

### Single Group (AND)
All rules must match:
```json
[
  {
    "logic": "and",
    "rules": [
      {"attribute": "geo.country", "operator": "equals", "values": [{"value": "Germany"}]},
      {"attribute": "time.hour", "operator": "greater_than", "values": [{"value": "9"}]}
    ]
  }
]
```
*Result: TRUE only if country is Germany AND hour > 9*

### Single Group (OR)
At least one rule must match:
```json
[
  {
    "logic": "or", 
    "rules": [
      {"attribute": "geo.city", "operator": "equals", "values": [{"value": "Berlin"}]},
      {"attribute": "geo.city", "operator": "equals", "values": [{"value": "Munich"}]}
    ]
  }
]
```
*Result: TRUE if city is Berlin OR Munich*

### Multiple Groups
All groups must evaluate to TRUE:
```json
[
  {
    "logic": "or",
    "rules": [
      {"attribute": "geo.country", "operator": "in", "values": [{"value": "Germany"}, {"value": "Austria"}]}
    ]
  },
  {
    "logic": "and", 
    "rules": [
      {"attribute": "attribute.premium_user", "operator": "is_true", "values": []}
    ]
  }
]
```
*Result: TRUE if (country is Germany OR Austria) AND user is premium*

## Best Practices

### Condition Design
1. **Start Simple**: Begin with basic conditions and add complexity gradually
2. **Test Thoroughly**: Verify conditions with various user contexts
3. **Document Logic**: Comment complex condition logic for team understanding
4. **Performance**: Avoid overly complex nested conditions

### Targeting Strategy
1. **Default Variant**: Always have a variant with no conditions as fallback
2. **Specific to General**: Order variants from most specific to most general
3. **User Experience**: Ensure conditions create coherent user experiences
4. **A/B Testing**: Use conditions to create controlled test groups

### Common Patterns
1. **Geographic Targeting**: Country/region-based content
2. **Time Targeting**: Business hours, seasonal, or time zone content
3. **User Segmentation**: Premium vs free, new vs returning users
4. **Device Targeting**: Mobile vs desktop experiences

## Troubleshooting

### Common Issues
- **Case Sensitivity**: String operators are case-sensitive
- **Data Types**: Ensure numeric comparisons use string values in JSON
- **Missing Attributes**: Handle cases where expected attributes don't exist
- **Logic Conflicts**: Avoid contradictory conditions within AND groups

### Testing Tips
1. **Mock Data**: Test conditions with sample user contexts
2. **Edge Cases**: Test with missing or unexpected attribute values
3. **Performance**: Monitor condition evaluation performance
4. **Documentation**: Keep examples of test contexts for each variant

---

Master conditions to create precise targeting rules that deliver the right content to the right users at the right time. 