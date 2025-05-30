# Getting Started with Testing

## Overview

The Usertune dashboard provides an intuitive testing interface that allows you to validate your Content containers and Variants with simulated user data. This guide will walk you through accessing and using the testing features.

## Accessing the Testing Feature

### Prerequisites

Before you can test your content, you need:
- A Usertune account with access to [app.usertune.io](https://app.usertune.io)
- A workspace created
- At least one content container with variants

### Step 1: Navigate to Your Content

1. **Sign in** to [app.usertune.io](https://app.usertune.io)
2. **Select your workspace** from the dashboard
3. **Click on a content container** you want to test
4. You'll see the content dashboard with your variants listed

### Step 2: Access the Testing Interface

Look for one of these options in your content dashboard:
- **"Test Content"** button
- **"Preview"** option
- **"Test"** tab or section
- **"Simulate"** feature

Click to open the testing panel where you can configure test scenarios.

## Using the Testing Interface

### Testing Panel Layout

The testing interface typically includes:

1. **Context Configuration Panel**: Where you set up fake user data
2. **Test Execution Controls**: Buttons to run tests
3. **Results Display**: Shows which variant was selected and the processed output
4. **Debug Information**: Details about condition evaluation (if available)

### Basic Testing Workflow

#### 1. Configure Test Context
Set up the simulated user data in the context configuration panel:

**Example Basic Configuration:**
```json
{
  "geo": {
    "country": "Germany",
    "city": "Berlin"
  },
  "attribute": {
    "user_name": "Anna",
    "premium_user": "true"
  }
}
```

#### 2. Run the Test
Click the **"Run Test"** or **"Execute"** button to process your test configuration.

#### 3. Review Results
The interface will show you:
- **Selected Variant**: Which variant matched your test conditions
- **Processed Payload**: The final content with variables replaced
- **Condition Evaluation**: How each variant's conditions were evaluated (if shown)

### Example Testing Session

Let's walk through a complete testing example:

#### Scenario: Testing a Welcome Message for German Users

**1. Set Up Test Context:**
```json
{
  "geo": {
    "country": "Germany",
    "city": "Munich"
  },
  "time": {
    "hour": "14",
    "weekday": "Tuesday"
  },
  "attribute": {
    "user_name": "Klaus",
    "premium_user": "false"
  }
}
```

**2. Expected Variant:**
Assuming you have a variant with conditions for German users, it should be selected.

**3. Expected Output:**
If your variant payload is:
```json
{
  "welcome": "Willkommen {attribute.user_name} aus {geo.city}!",
  "offer": "Spezielle Angebote für Deutschland"
}
```

The processed result should be:
```json
{
  "welcome": "Willkommen Klaus aus Munich!",
  "offer": "Spezielle Angebote für Deutschland"
}
```

## Quick Testing Tips

### Start Simple
Begin with basic test cases:
```json
{
  "geo": {"country": "United States"},
  "attribute": {"user_name": "John"}
}
```

### Test One Thing at a Time
- Test geographic targeting first
- Then add time conditions
- Finally test complex attribute logic

### Use Realistic Data
- Use real country and city names
- Use proper time formats
- Use realistic user attributes

### Test Edge Cases
- Empty values: `{"geo": {"country": ""}}`
- Missing data: `{"attribute": {}}`
- Unexpected values: `{"time": {"hour": "25"}}`

## Common Testing Scenarios

### New User from Unknown Location
```json
{
  "geo": {
    "country": "",
    "city": ""
  },
  "attribute": {
    "first_visit": "true",
    "premium_user": "false"
  }
}
```

### Premium User During Business Hours
```json
{
  "geo": {
    "country": "United States",
    "city": "San Francisco"
  },
  "time": {
    "hour": "10",
    "weekday": "Monday"
  },
  "attribute": {
    "premium_user": "true",
    "user_name": "Sarah"
  }
}
```

### Weekend User
```json
{
  "time": {
    "hour": "15",
    "weekday": "Saturday"
  },
  "attribute": {
    "user_name": "Mike"
  }
}
```

## Troubleshooting Common Issues

### No Variant Selected
**Problem**: The test returns no variant or an error.

**Solutions:**
- Check that at least one variant is enabled (`is_enabled: true`)
- Verify that your content container is published
- Create a default variant with no conditions as a fallback

### Wrong Variant Selected
**Problem**: A different variant than expected is chosen.

**Solutions:**
- Review the conditions of the selected variant
- Check for typos in condition values
- Verify the logic operators (AND vs OR)
- Consider variant priority or order

### Variables Not Replaced
**Problem**: Variables like `{geo.city}` appear unchanged in the output.

**Solutions:**
- Ensure your test context includes the required data
- Check variable name spelling and syntax
- Verify the variable path matches your context structure

## Next Steps

Once you're comfortable with basic testing:

1. **Learn Context Configuration**: Explore [Context Configuration](context-configuration.md) for detailed information about setting up test data
2. **Try Different Scenarios**: Check out [Test Scenarios](scenarios.md) for more complex testing examples
3. **Master Best Practices**: Read [Best Practices](best-practices.md) for advanced testing techniques

---

Testing is your safety net for personalization. Use it frequently to ensure your content works as expected across all user scenarios. 