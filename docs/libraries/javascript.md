# JavaScript Library (usertune.js)

**usertune.js** is the official JavaScript/TypeScript client library for Usertune's API. It provides a modern, type-safe interface that works seamlessly in both Node.js and browser environments.

[![npm version](https://badge.fury.io/js/usertune.js.svg)](https://badge.fury.io/js/usertune.js)
[![TypeScript](https://img.shields.io/badge/%3C%2F%3E-TypeScript-%230074c1.svg)](http://www.typescriptlang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 🚀 Features

- **🎯 Simple API** - Clean, intuitive interface with just two main methods
- **🔒 Type-Safe** - Full TypeScript support with comprehensive type definitions
- **⚡ Modern** - ES modules, async/await, built for modern JavaScript
- **🌐 Universal** - Works in Node.js and browsers
- **🎛️ Flexible** - Support for custom attributes and personalization
- **📦 Zero Config** - Works out of the box with sensible defaults
- **🔄 Smart Tracking** - Automatic variant ID management for seamless conversion tracking

## Installation

=== "npm"

    ```bash
    npm install usertune.js
    ```

=== "yarn"

    ```bash
    yarn add usertune.js
    ```

=== "pnpm"

    ```bash
    pnpm add usertune.js
    ```

## Quick Start

### Basic Usage

```javascript
import { Usertune } from 'usertune.js';

// For public content (no authentication required)
const client = new Usertune({
  workspace: 'your-workspace-id'
});

const content = await client.content('public-banner');
console.log(content.data);

// Track conversions
await client.track('purchase', 99.99);
```

### CDN Usage (Browser)

You can also use usertune.js directly in the browser via CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/usertune.js@latest/dist/usertune.browser.min.js"></script>
<script>
  const client = new Usertune({
    workspace: 'your-workspace-id'
  });
  
  client.content('banner-content').then(content => {
    console.log(content.data);
  });
</script>
```

## Configuration

### Constructor Options

```javascript
const client = new Usertune({
  workspace: 'your-workspace-id',      // Required: Your workspace identifier
  accessToken: 'your-access-token',    // Optional: For private content
  baseUrl: 'https://api.usertune.io',  // Optional: Custom API base URL
  timeout: 10000,                      // Optional: Request timeout (ms)
  debug: false                         // Optional: Enable debug logging
});
```

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `workspace` | `string` | ✅ | - | Your Usertune workspace identifier |
| `accessToken` | `string` | ❌ | - | API access token (required for private content) |
| `baseUrl` | `string` | ❌ | `https://api.usertune.io` | Custom API base URL |
| `timeout` | `number` | ❌ | `10000` | Request timeout in milliseconds |
| `debug` | `boolean` | ❌ | `false` | Enable debug logging |

## API Methods

### content(contentSlug, attributes?)

Retrieve personalized content for a user.

```javascript
const content = await client.content('banner-content', {
  user_tier: 'premium',
  location: 'berlin',
  age: 28
});

console.log(content.data.title);
console.log(content.metadata.variant_id);
```

**Parameters:**

- `contentSlug` (`string`): The content piece identifier
- `attributes` (`object`, optional): Custom attributes for personalization

**Returns:** `ContentResponse`

```typescript
interface ContentResponse {
  data: {
    [key: string]: any;
  };
  metadata: {
    variant_id: string | null;
    [key: string]: any;
  };
}
```

### track(conversionType, conversionValue?)

Track a conversion event. Must be called after `content()` to have a variant ID.

```javascript
await client.track('purchase', 75.99);
await client.track('signup'); // Value is optional
```

**Parameters:**

- `conversionType` (`string`): Type of conversion (e.g., 'purchase', 'signup', 'click')
- `conversionValue` (`number`, optional): Monetary value of the conversion

### contentWithTracker(contentSlug, attributes?)

Convenience method that returns both content and a bound tracking function.

```javascript
const { content, track } = await client.contentWithTracker('popup-offer', {
  user_id: 'user-123'
});

console.log(content.data.title);

// Later, when user converts...
await track('signup');
```

## Usage Examples

### Public Content Access

For content that doesn't require authentication:

```javascript
import { Usertune } from 'usertune.js';

const client = new Usertune({
  workspace: 'my-workspace'
});

const content = await client.content('public-banner');
console.log(content.data);
```

### Private Content Access

For personalized content that requires authentication:

```javascript
import { Usertune } from 'usertune.js';

const client = new Usertune({
  workspace: 'my-workspace',
  accessToken: 'your-access-token'
});

const content = await client.content('personalized-banner', {
  user_tier: 'premium'
});
console.log(content.data);
```

### Personalized Content with Attributes

```javascript
const content = await client.content('product-recommendation', {
  user_tier: 'premium',
  purchase_history: 'electronics',
  location: 'us-west',
  age_group: '25-34'
});

// Use the personalized content
displayRecommendations(content.data);
```

### Complete Flow with Conversion Tracking

```javascript
// Get personalized content
const content = await client.content('checkout-banner', {
  cart_value: 150,
  user_segment: 'high-value'
});

// Display content to user
displayBanner(content.data);

// Track conversion when user completes purchase
if (userCompletedPurchase) {
  await client.track('purchase', 150.00);
}
```

### Using contentWithTracker for Convenience

```javascript
const { content, track } = await client.contentWithTracker('email-signup-form', {
  traffic_source: 'google',
  page_type: 'landing'
});

// Display the form
showSignupForm(content.data);

// Track different conversion events
document.getElementById('signup-btn').onclick = () => track('signup');
document.getElementById('newsletter-btn').onclick = () => track('newsletter_signup');
```

## Error Handling

The library provides detailed error information for debugging:

```javascript
try {
  const content = await client.content('my-content');
  console.log(content);
} catch (error) {
  if (error.status === 404) {
    console.log('Content not found');
  } else if (error.status === 401) {
    console.log('Invalid access token');
  } else {
    console.log('Request failed:', error.message);
  }
}
```

## TypeScript Support

The library is written in TypeScript and provides comprehensive type definitions:

```typescript
import { Usertune, ContentResponse, UsertuneConfig } from 'usertune.js';

const config: UsertuneConfig = {
  workspace: 'my-workspace',
  accessToken: 'token'
};

const client = new Usertune(config);

const content: ContentResponse = await client.content('banner', {
  userId: 'user-123'
});
```

## Requirements

- **Node.js**: >= 14.0.0
- **Browsers**: Modern browsers with ES2020 support

## Browser Compatibility

The library works in all modern browsers that support:

- ES2020 features
- Fetch API
- Promises/async-await

For older browsers, you may need polyfills for these features.

## Performance Considerations

- The library uses modern JavaScript features for optimal performance
- Automatic request deduplication prevents unnecessary API calls
- Lightweight bundle size optimized for both Node.js and browsers
- Smart caching of variant IDs for efficient conversion tracking

## Contributing

The library is open source and welcomes contributions:

1. Fork the [repository](https://github.com/usertune/usertune.js)
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Links

- **GitHub Repository**: [https://github.com/usertune/usertune.js](https://github.com/usertune/usertune.js)
- **npm Package**: [https://npmjs.com/package/usertune.js](https://npmjs.com/package/usertune.js)
- **CDN Documentation**: [View CDN Usage Guide](https://github.com/usertune/usertune.js/blob/main/CDN.md)

## License

This library is licensed under the [MIT License](https://github.com/usertune/usertune.js/blob/main/LICENSE). 