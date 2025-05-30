# Testing Best Practices

## Overview

Effective testing is crucial for building reliable personalization experiences. This guide covers best practices, workflows, and troubleshooting techniques to help you create robust and validated content delivery systems.

## Testing Workflow

### Step 1: Test Condition Matching

**Purpose**: Verify that variants are selected correctly based on your conditions.

#### Process:
1. **Set up test context** with specific values that should match your variant conditions
2. **Run the test** to see which variant is selected
3. **Verify the result** matches your expectations
4. **Adjust conditions** if the wrong variant is selected

#### Example Testing Flow:
```json
// Test Context
{
  "geo": {"country": "Germany", "city": "Berlin"},
  "attribute": {"premium_user": "true"}
}

// Expected: German Premium Variant selected
// Actual: Check test results
// Action: Adjust conditions if needed
```

### Step 2: Test Payload Processing

**Purpose**: Ensure payload variables are replaced correctly with context data.

#### Process:
1. **Check variable replacement** in the selected variant's payload
2. **Verify dynamic content** appears as expected
3. **Test missing variables** to ensure graceful handling
4. **Validate HTML rendering** if using HTML content

#### Example Validation:
```json
// Original Payload
{
  "greeting": "Hello {attribute.user_name} from {geo.city}!"
}

// Expected Output
{
  "greeting": "Hello Anna from Berlin!"
}

// Verify: Variables are replaced with actual context values
```

### Step 3: Test Edge Cases

**Purpose**: Ensure robust behavior in unusual scenarios.

#### Edge Cases to Test:
1. **Missing attributes**: Incomplete context data
2. **Invalid values**: Unexpected or malformed data
3. **Empty context**: Minimal or no context data
4. **Boundary conditions**: Edge cases for numeric comparisons

## Testing Best Practices

### 1. Test Early and Often

#### Development Testing
- **Test during development**: Don't wait until variants are complete
- **Iterative testing**: Test after each condition or payload change
- **Quick validation**: Use simple test cases for rapid feedback

#### Continuous Testing
- **Regression testing**: Re-test when modifying existing variants
- **Integration testing**: Test how variants work together
- **Performance testing**: Monitor response times with complex conditions

### 2. Create Comprehensive Test Cases

#### Condition Coverage
- **Test all paths**: Ensure every condition branch is tested
- **Logical combinations**: Test AND/OR logic combinations
- **Operator validation**: Test each operator type (equals, contains, etc.)

#### Data Coverage
- **Multiple scenarios**: Test various combinations of context data
- **Real data simulation**: Use realistic test values
- **Edge cases**: Include boundary conditions and error scenarios
- **Scale testing**: Test with large numbers of variants

#### Example Test Matrix:
```
Geographic Tests:
✓ US users
✓ European users
✓ Unknown location
✓ Partial location data

Time Tests:
✓ Business hours
✓ After hours  
✓ Weekends
✓ Holidays

User Segment Tests:
✓ Premium users
✓ Free users
✓ New users
✓ Returning users
```

### 3. Document Your Tests

#### Test Documentation
- **Save test scenarios**: Keep successful test configurations
- **Document expected results**: Note what should happen for each test
- **Version control**: Track test changes with your variants
- **Share with team**: Ensure team members understand test cases

#### Test Case Template:
```markdown
## Test Case: Premium User Germany

**Context:**
```json
{
  "geo": {"country": "Germany"},
  "attribute": {"premium_user": "true"}
}
```

**Expected Variant:** German Premium Welcome

**Expected Output:**
- Greeting in German
- Premium features highlighted
- Local contact information

**Last Updated:** 2024-03-15
**Status:** ✅ Passing
```

### 4. Validate Business Logic

#### User Journey Testing
- **Complete flows**: Test entire user experience paths
- **Transition points**: Test condition changes over time
- **Fallback scenarios**: Ensure default variants work properly

#### Segmentation Accuracy
- **Target audiences**: Verify user groups are targeted correctly
- **Exclusion logic**: Test that users are properly excluded when appropriate
- **Overlap handling**: Test what happens when multiple variants could match

#### Personalization Validation
- **Content accuracy**: Ensure content matches user context
- **Variable processing**: Verify all dynamic elements work correctly
- **Consistency**: Test that personalization feels coherent across sessions

## Advanced Testing Techniques

### Systematic Testing Approach

#### 1. Baseline Testing
Start with the simplest possible test cases:

```json
// Baseline: Minimal context
{
  "geo": {"country": "United States"}
}

// Verify: Basic geographic targeting works
```

#### 2. Progressive Complexity
Gradually add more complex conditions:

```json
// Step 1: Add time
{
  "geo": {"country": "United States"},
  "time": {"hour": "14"}
}

// Step 2: Add user attributes  
{
  "geo": {"country": "United States"},
  "time": {"hour": "14"},
  "attribute": {"premium_user": "true"}
}
```

#### 3. Boundary Testing
Test edge cases and limits:

```json
// Time boundaries
{"time": {"hour": "0"}}    // Midnight
{"time": {"hour": "23"}}   // Last hour
{"time": {"hour": "24"}}   // Invalid hour

// Empty values
{"geo": {"country": ""}}
{"attribute": {"user_name": ""}}
```

### Load and Performance Testing

#### Context Complexity
- **Simple context**: Basic geo + attribute data
- **Complex context**: Multiple attributes and conditions
- **Large payloads**: Test with extensive payload data

#### Variant Scale Testing
- **Few variants**: 2-3 variants per content
- **Many variants**: 10+ variants with complex conditions
- **Deep nesting**: Complex condition logic trees

### A/B Testing Validation

#### Split Testing
```json
// Control group
{
  "attribute": {
    "ab_test_group": "control",
    "user_id": "12345"
  }
}

// Variant group
{
  "attribute": {
    "ab_test_group": "variant_a", 
    "user_id": "67890"
  }
}
```

#### Statistical Validation
- **Random distribution**: Verify random assignment works
- **Consistent assignment**: Same user gets same variant
- **Group balance**: Check for even distribution

## Troubleshooting Common Issues

### No Variant Selected

**Symptoms:**
- Test returns empty result
- Error message about no matching variant
- Default content not appearing

**Possible Causes:**
- All variants are disabled (`is_enabled: false`)
- No variant conditions match the test context
- Content container is not published
- Malformed condition syntax

**Solutions:**
1. **Check variant status**: Ensure at least one variant is enabled
2. **Review conditions**: Verify condition logic and values
3. **Create fallback**: Add a default variant with no conditions
4. **Validate syntax**: Check condition JSON for errors

**Debug Steps:**
```json
// 1. Test with minimal context
{"attribute": {"user_name": "test"}}

// 2. Check each variant's conditions individually
// 3. Verify content container is published
// 4. Test with a completely open variant (no conditions)
```

### Wrong Variant Selected

**Symptoms:**
- Different variant than expected is chosen
- Conditions seem correct but wrong result
- Inconsistent variant selection

**Possible Causes:**
- Condition logic is incorrect (AND vs OR)
- Case sensitivity in string comparisons
- Multiple variants match (first match wins)
- Operator misuse (equals vs contains)

**Solutions:**
1. **Review condition operators**: Verify correct operators are used
2. **Check case sensitivity**: Ensure string values match exactly
3. **Reorder variants**: Place more specific variants first
4. **Test conditions individually**: Isolate each condition

**Debug Example:**
```json
// Problem: User in "New York" not matching city condition
{
  "geo": {"city": "New York"},
  "attribute": {"test": "true"}
}

// Check condition operator:
// Wrong: "equals" with "new york" (case mismatch)
// Right: "equals" with "New York" (exact match)
// Alternative: "contains" with "New" (partial match)
```

### Variables Not Replaced

**Symptoms:**
- Variables like `{geo.city}` appear unchanged in output
- Dynamic content shows placeholder text
- Personalization not working

**Possible Causes:**
- Missing context data for variables
- Typos in variable names
- Incorrect variable syntax
- Variable path doesn't match context structure

**Solutions:**
1. **Verify context data**: Ensure test context includes required data
2. **Check variable syntax**: Use correct `{context.field}` format
3. **Match context structure**: Ensure variable paths are correct
4. **Test variable processing**: Use simple variables first

**Debug Process:**
```json
// Test Context
{
  "geo": {"city": "Berlin"},
  "attribute": {"user_name": "Anna"}
}

// Payload Template
{
  "message": "Hello {attribute.user_name} from {geo.city}!"
}

// Expected Output
{
  "message": "Hello Anna from Berlin!"
}

// If not working, check:
// 1. Context has "attribute.user_name" and "geo.city"
// 2. Variable syntax is correct: {attribute.user_name}
// 3. No typos in variable names
```

### Unexpected Output

**Symptoms:**
- Content appears corrupted or malformed
- HTML rendering issues
- Character encoding problems

**Possible Causes:**
- HTML encoding issues
- Character limit problems
- Data type mismatches
- Special characters in content

**Solutions:**
1. **Test with simple data**: Use basic ASCII characters
2. **Check HTML validation**: Ensure HTML is well-formed
3. **Verify encoding**: Test with international characters
4. **Validate JSON structure**: Ensure proper JSON formatting

## Testing Automation

### API Testing Scripts

Create reusable test scripts for common scenarios:

```javascript
// Basic test function
async function testVariant(context, expectedVariant) {
  const response = await fetch('/v1/workspace/123/content/test', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(context)
  });
  
  const result = await response.json();
  console.log(`Expected: ${expectedVariant}, Got: ${result.variant_name}`);
  return result;
}

// Test suite
const testCases = [
  {
    name: "German Premium User",
    context: {
      geo: { country: "Germany" },
      attribute: { premium_user: "true" }
    },
    expected: "German Premium Variant"
  },
  // Add more test cases...
];

// Run all tests
testCases.forEach(test => {
  testVariant(test.context, test.expected);
});
```

### Continuous Integration

Integrate testing into your development workflow:

1. **Pre-deployment testing**: Run tests before publishing variants
2. **Automated regression testing**: Test existing variants when adding new ones
3. **Performance monitoring**: Track test execution times
4. **Alert on failures**: Get notified when tests fail

## Quality Assurance Checklist

### Before Publishing

- [ ] All variants have been tested individually
- [ ] Condition logic has been validated
- [ ] Variable replacement works correctly
- [ ] Edge cases have been tested
- [ ] Fallback variants are in place
- [ ] Performance is acceptable

### Regular Maintenance

- [ ] Re-test variants after condition changes
- [ ] Update test cases when adding new variants
- [ ] Monitor production performance
- [ ] Review and update test documentation
- [ ] Clean up unused test configurations

---

Thorough testing ensures your personalization works correctly across all user scenarios. Follow these best practices to build confidence in your content delivery and create reliable user experiences. 