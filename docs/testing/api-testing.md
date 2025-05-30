# API Testing

## Overview

API testing allows you to programmatically validate your Content and Variants using custom context data. This is essential for integration testing, automated workflows, and validating content behavior in your application environment.

## API Testing Basics

### Content Request API

The core API endpoint for testing content delivery:

```
POST /v1/workspace/{workspace_id}/content/{content_slug}
```

### Request Format

Send context data as JSON in the request body:

```javascript
{
  "geo": {
    "country": "Germany",
    "city": "Berlin"
  },
  "time": {
    "hour": "14",
    "weekday": "Monday"
  },
  "attribute": {
    "user_name": "Anna",
    "premium_user": "true"
  }
}
```

### Response Format

The API returns the selected variant's processed payload:

```javascript
{
  "variant_id": "123",
  "variant_name": "German Premium Welcome",
  "payload": {
    "greeting": "Willkommen Anna aus Berlin!",
    "offer": "Premium features available"
  },
  "processing_time": "12ms"
}
```

## JavaScript Testing Examples

### Basic API Test

```javascript
async function testContent(workspaceId, contentSlug, context) {
  try {
    const response = await fetch(`/v1/workspace/${workspaceId}/content/${contentSlug}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer YOUR_API_KEY' // If required
      },
      body: JSON.stringify(context)
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const result = await response.json();
    console.log('Selected variant:', result.variant_name);
    console.log('Processed payload:', result.payload);
    return result;
  } catch (error) {
    console.error('Test failed:', error);
    throw error;
  }
}

// Example usage
const context = {
  geo: { country: "Germany", city: "Berlin" },
  attribute: { premium_user: "true" }
};

testContent('workspace-123', 'homepage-hero', context);
```

### Test Suite Framework

```javascript
class ContentTestSuite {
  constructor(workspaceId, baseUrl = '') {
    this.workspaceId = workspaceId;
    this.baseUrl = baseUrl;
    this.results = [];
  }

  async runTest(testCase) {
    const { name, contentSlug, context, expected } = testCase;
    
    try {
      const result = await this.testContent(contentSlug, context);
      const passed = this.validateResult(result, expected);
      
      this.results.push({
        name,
        passed,
        result,
        expected,
        timestamp: new Date().toISOString()
      });

      console.log(`${passed ? '✅' : '❌'} ${name}`);
      return { passed, result };
    } catch (error) {
      console.error(`❌ ${name} - Error:`, error.message);
      this.results.push({
        name,
        passed: false,
        error: error.message,
        timestamp: new Date().toISOString()
      });
      return { passed: false, error };
    }
  }

  async testContent(contentSlug, context) {
    const response = await fetch(`${this.baseUrl}/v1/workspace/${this.workspaceId}/content/${contentSlug}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(context)
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return await response.json();
  }

  validateResult(result, expected) {
    if (expected.variant_name && result.variant_name !== expected.variant_name) {
      return false;
    }
    
    if (expected.payload) {
      for (const [key, value] of Object.entries(expected.payload)) {
        if (result.payload[key] !== value) {
          return false;
        }
      }
    }
    
    return true;
  }

  async runAllTests(testCases) {
    console.log(`Running ${testCases.length} tests...`);
    
    for (const testCase of testCases) {
      await this.runTest(testCase);
    }
    
    const passed = this.results.filter(r => r.passed).length;
    const total = this.results.length;
    
    console.log(`\n📊 Results: ${passed}/${total} tests passed`);
    return this.results;
  }

  getReport() {
    return {
      summary: {
        total: this.results.length,
        passed: this.results.filter(r => r.passed).length,
        failed: this.results.filter(r => !r.passed).length
      },
      details: this.results
    };
  }
}
```

### Example Test Cases

```javascript
const testCases = [
  {
    name: "German Premium User",
    contentSlug: "homepage-hero",
    context: {
      geo: { country: "Germany", city: "Berlin" },
      attribute: { premium_user: "true", user_name: "Anna" }
    },
    expected: {
      variant_name: "German Premium Welcome",
      payload: {
        greeting: "Willkommen Anna aus Berlin!"
      }
    }
  },
  {
    name: "US Free User",
    contentSlug: "homepage-hero", 
    context: {
      geo: { country: "United States", city: "New York" },
      attribute: { premium_user: "false", user_name: "John" }
    },
    expected: {
      variant_name: "US Free User Welcome"
    }
  },
  {
    name: "Unknown Location Fallback",
    contentSlug: "homepage-hero",
    context: {
      geo: { country: "", city: "" },
      attribute: { user_name: "Anonymous" }
    },
    expected: {
      variant_name: "Default Welcome"
    }
  }
];

// Run the test suite
const testSuite = new ContentTestSuite('workspace-123');
testSuite.runAllTests(testCases).then(results => {
  console.log('Final report:', testSuite.getReport());
});
```

## Node.js Testing Examples

### Simple Node.js Test

```javascript
const https = require('https');

function testContentNodeJS(workspaceId, contentSlug, context) {
  return new Promise((resolve, reject) => {
    const data = JSON.stringify(context);
    
    const options = {
      hostname: 'api.usertune.io',
      path: `/v1/workspace/${workspaceId}/content/${contentSlug}`,
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Content-Length': data.length
      }
    };

    const req = https.request(options, (res) => {
      let responseData = '';
      
      res.on('data', (chunk) => {
        responseData += chunk;
      });
      
      res.on('end', () => {
        try {
          const result = JSON.parse(responseData);
          resolve(result);
        } catch (error) {
          reject(new Error('Invalid JSON response'));
        }
      });
    });

    req.on('error', (error) => {
      reject(error);
    });

    req.write(data);
    req.end();
  });
}

// Example usage
testContentNodeJS('workspace-123', 'homepage-hero', {
  geo: { country: 'Germany' },
  attribute: { premium_user: 'true' }
}).then(result => {
  console.log('Result:', result);
}).catch(error => {
  console.error('Error:', error);
});
```

### Express.js Integration Testing

```javascript
const express = require('express');
const request = require('supertest');

// Your app setup
const app = express();
app.use(express.json());

// Endpoint that uses Usertune
app.post('/personalized-content', async (req, res) => {
  const { userId, location } = req.body;
  
  // Build context from request
  const context = {
    geo: { 
      country: location.country,
      city: location.city 
    },
    attribute: {
      user_id: userId,
      premium_user: req.user?.isPremium ? 'true' : 'false'
    }
  };

  try {
    // Call Usertune API
    const response = await fetch('/v1/workspace/123/content/homepage-hero', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(context)
    });

    const result = await response.json();
    res.json(result.payload);
  } catch (error) {
    res.status(500).json({ error: 'Content service unavailable' });
  }
});

// Integration tests
describe('Personalized Content API', () => {
  test('should return German content for German users', async () => {
    const response = await request(app)
      .post('/personalized-content')
      .send({
        userId: 'user123',
        location: { country: 'Germany', city: 'Berlin' }
      });

    expect(response.status).toBe(200);
    expect(response.body.greeting).toContain('Willkommen');
  });

  test('should return default content for unknown location', async () => {
    const response = await request(app)
      .post('/personalized-content')
      .send({
        userId: 'user456',
        location: { country: '', city: '' }
      });

    expect(response.status).toBe(200);
    expect(response.body.greeting).toContain('Welcome');
  });
});
```

## Python Testing Examples

### Basic Python Test

```python
import requests
import json

def test_content(workspace_id, content_slug, context):
    """Test content API with given context"""
    url = f"https://api.usertune.io/v1/workspace/{workspace_id}/content/{content_slug}"
    
    headers = {
        'Content-Type': 'application/json'
    }
    
    try:
        response = requests.post(url, json=context, headers=headers)
        response.raise_for_status()
        
        result = response.json()
        print(f"Selected variant: {result.get('variant_name')}")
        print(f"Payload: {result.get('payload')}")
        return result
        
    except requests.exceptions.RequestException as e:
        print(f"Test failed: {e}")
        raise

# Example usage
context = {
    "geo": {"country": "Germany", "city": "Berlin"},
    "attribute": {"premium_user": "true"}
}

result = test_content('workspace-123', 'homepage-hero', context)
```

### Python Test Suite with pytest

```python
import pytest
import requests
from typing import Dict, Any

class UsertuneTestClient:
    def __init__(self, workspace_id: str, base_url: str = "https://api.usertune.io"):
        self.workspace_id = workspace_id
        self.base_url = base_url
    
    def test_content(self, content_slug: str, context: Dict[str, Any]) -> Dict[str, Any]:
        """Send test request to Usertune API"""
        url = f"{self.base_url}/v1/workspace/{self.workspace_id}/content/{content_slug}"
        
        response = requests.post(url, json=context)
        response.raise_for_status()
        
        return response.json()

@pytest.fixture
def client():
    return UsertuneTestClient('workspace-123')

class TestGeographicTargeting:
    def test_german_users(self, client):
        context = {
            "geo": {"country": "Germany", "city": "Berlin"},
            "attribute": {"premium_user": "true"}
        }
        
        result = client.test_content('homepage-hero', context)
        
        assert result['variant_name'] == 'German Premium Welcome'
        assert 'Willkommen' in result['payload']['greeting']
    
    def test_us_users(self, client):
        context = {
            "geo": {"country": "United States", "city": "New York"},
            "attribute": {"premium_user": "false"}
        }
        
        result = client.test_content('homepage-hero', context)
        
        assert result['variant_name'] == 'US Free User Welcome'
        assert 'Welcome' in result['payload']['greeting']

class TestUserSegmentation:
    def test_premium_users(self, client):
        context = {
            "geo": {"country": "Canada"},
            "attribute": {"premium_user": "true", "user_name": "Sarah"}
        }
        
        result = client.test_content('homepage-hero', context)
        
        assert 'premium' in result['variant_name'].lower()
        assert 'Sarah' in result['payload']['greeting']
    
    def test_free_users(self, client):
        context = {
            "geo": {"country": "Canada"},
            "attribute": {"premium_user": "false", "user_name": "John"}
        }
        
        result = client.test_content('homepage-hero', context)
        
        assert 'free' in result['variant_name'].lower()
        assert 'upgrade' in result['payload'].get('cta', '').lower()

# Run tests
# pytest test_usertune.py -v
```

## Performance Testing

### Load Testing with Artillery

Create an `artillery.yml` file:

```yaml
config:
  target: 'https://api.usertune.io'
  phases:
    - duration: 60
      arrivalRate: 10
  variables:
    workspace_id: 'workspace-123'
    content_slug: 'homepage-hero'

scenarios:
  - name: "Test German Users"
    weight: 40
    flow:
      - post:
          url: "/v1/workspace/{{ workspace_id }}/content/{{ content_slug }}"
          json:
            geo:
              country: "Germany"
              city: "Berlin"
            attribute:
              premium_user: "true"
              user_name: "TestUser"

  - name: "Test US Users"
    weight: 40
    flow:
      - post:
          url: "/v1/workspace/{{ workspace_id }}/content/{{ content_slug }}"
          json:
            geo:
              country: "United States"
              city: "New York"
            attribute:
              premium_user: "false"
              user_name: "TestUser"

  - name: "Test Unknown Location"
    weight: 20
    flow:
      - post:
          url: "/v1/workspace/{{ workspace_id }}/content/{{ content_slug }}"
          json:
            geo:
              country: ""
              city: ""
            attribute:
              user_name: "Anonymous"
```

Run with: `artillery run artillery.yml`

### Response Time Testing

```javascript
async function measureResponseTime(workspaceId, contentSlug, context, iterations = 100) {
  const times = [];
  
  for (let i = 0; i < iterations; i++) {
    const start = Date.now();
    
    try {
      await testContent(workspaceId, contentSlug, context);
      const end = Date.now();
      times.push(end - start);
    } catch (error) {
      console.error(`Iteration ${i} failed:`, error.message);
    }
  }
  
  const avg = times.reduce((a, b) => a + b, 0) / times.length;
  const min = Math.min(...times);
  const max = Math.max(...times);
  
  console.log(`Response Times (${iterations} requests):`);
  console.log(`Average: ${avg.toFixed(2)}ms`);
  console.log(`Min: ${min}ms`);
  console.log(`Max: ${max}ms`);
  
  return { avg, min, max, times };
}

// Test response times
measureResponseTime('workspace-123', 'homepage-hero', {
  geo: { country: 'Germany' },
  attribute: { premium_user: 'true' }
});
```

## Continuous Integration

### GitHub Actions Example

```yaml
name: Usertune Content Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'
    
    - name: Install dependencies
      run: npm install
    
    - name: Run Usertune Content Tests
      env:
        USERTUNE_WORKSPACE_ID: ${{ secrets.USERTUNE_WORKSPACE_ID }}
      run: |
        node test-content.js
    
    - name: Upload test results
      uses: actions/upload-artifact@v2
      if: always()
      with:
        name: test-results
        path: test-results.json
```

## Error Handling and Debugging

### Common API Errors

```javascript
async function robustContentTest(workspaceId, contentSlug, context) {
  try {
    const response = await fetch(`/v1/workspace/${workspaceId}/content/${contentSlug}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(context)
    });

    // Handle different HTTP status codes
    switch (response.status) {
      case 200:
        return await response.json();
      
      case 404:
        throw new Error(`Content not found: ${contentSlug}`);
      
      case 400:
        const errorData = await response.json();
        throw new Error(`Invalid request: ${errorData.message}`);
      
      case 403:
        throw new Error('Access forbidden - check API credentials');
      
      case 500:
        throw new Error('Server error - try again later');
      
      default:
        throw new Error(`Unexpected status: ${response.status}`);
    }
  } catch (error) {
    if (error instanceof TypeError) {
      throw new Error('Network error - check connection');
    }
    throw error;
  }
}
```

### Debug Logging

```javascript
function debugContentTest(workspaceId, contentSlug, context) {
  console.log('🔍 Debug Info:');
  console.log('Workspace ID:', workspaceId);
  console.log('Content Slug:', contentSlug);
  console.log('Context:', JSON.stringify(context, null, 2));
  
  return testContent(workspaceId, contentSlug, context)
    .then(result => {
      console.log('✅ Success:');
      console.log('Response:', JSON.stringify(result, null, 2));
      return result;
    })
    .catch(error => {
      console.error('❌ Error:');
      console.error('Message:', error.message);
      console.error('Stack:', error.stack);
      throw error;
    });
}
```

---

API testing ensures your personalization logic works correctly in your application environment. Use these patterns to build comprehensive test suites that validate content delivery across all scenarios. 