# Geographic Attributes

## Overview

Usertune automatically provides rich geographic and device information for every request through the `geo.*` attribute namespace. These attributes are extracted from CloudFront headers and enable powerful location-based and device-based personalization.

## Available Geographic Attributes

All geographic attributes are accessible in variant conditions and payloads using the `geo.` prefix.

### Location Attributes

#### `geo.country`
- **Description**: Two-letter country code (ISO 3166-1 alpha-2)
- **Format**: Uppercase country code
- **Examples**: `"US"`, `"DE"`, `"GB"`, `"JP"`, `"CA"`
- **Use Cases**: Country-specific content, regional compliance, language selection

```json
{
  "attribute": "geo.country",
  "operator": "equals",
  "values": [{"value": "DE"}]
}
```

#### `geo.country_name`
- **Description**: Full country name in English
- **Format**: Human-readable country name
- **Examples**: `"United States"`, `"Germany"`, `"United Kingdom"`, `"Japan"`, `"Canada"`
- **Use Cases**: Display country name in content, user-friendly messaging

```json
{
  "greeting": "Welcome to our {geo.country_name} store!"
}
```

#### `geo.region`
- **Description**: State/province/region code
- **Format**: Region abbreviation or code
- **Examples**: `"CA"` (California), `"NY"` (New York), `"ON"` (Ontario), `"BY"` (Bavaria)
- **Use Cases**: State-specific promotions, regional content, local regulations

```json
{
  "attribute": "geo.region",
  "operator": "in",
  "values": [{"value": "CA"}, {"value": "NY"}, {"value": "TX"}]
}
```

#### `geo.region_name`
- **Description**: Full region/state/province name
- **Format**: Human-readable region name
- **Examples**: `"California"`, `"New York"`, `"Ontario"`, `"Bavaria"`
- **Use Cases**: Regional targeting, location-based messaging

```json
{
  "message": "Special offers for {geo.region_name} residents!"
}
```

#### `geo.city`
- **Description**: City name where the user is located
- **Format**: City name as determined by IP geolocation
- **Examples**: `"San Francisco"`, `"Berlin"`, `"London"`, `"Tokyo"`
- **Use Cases**: City-specific content, local events, store locations

```json
{
  "attribute": "geo.city",
  "operator": "contains",
  "values": [{"value": "San"}]
}
```

#### `geo.postal_code`
- **Description**: Postal/ZIP code of the user's location
- **Format**: Postal code format varies by country
- **Examples**: `"94102"`, `"10001"`, `"SW1A 1AA"`, `"100-0001"`
- **Use Cases**: Hyperlocal targeting, shipping zones, service areas

```json
{
  "attribute": "geo.postal_code",
  "operator": "starts_with",
  "values": [{"value": "100"}]
}
```

### Coordinate Attributes

#### `geo.latitude`
- **Description**: Geographic latitude coordinate
- **Format**: Decimal degrees as string
- **Examples**: `"37.7749"`, `"52.5200"`, `"51.5074"`
- **Use Cases**: Distance calculations, radius-based targeting, map displays

```json
{
  "attribute": "geo.latitude",
  "operator": "greater_than",
  "values": [{"value": "40"}]
}
```

#### `geo.longitude`
- **Description**: Geographic longitude coordinate
- **Format**: Decimal degrees as string
- **Examples**: `"-122.4194"`, `"13.4050"`, `"-0.1278"`
- **Use Cases**: Distance calculations, radius-based targeting, map displays

```json
{
  "location": "Your coordinates: {geo.latitude}, {geo.longitude}"
}
```

### Time Zone Attributes

#### `geo.time_zone`
- **Description**: User's time zone identifier
- **Format**: IANA time zone identifier
- **Examples**: `"America/Los_Angeles"`, `"Europe/Berlin"`, `"Asia/Tokyo"`
- **Use Cases**: Time-aware content, business hours, event scheduling

```json
{
  "attribute": "geo.time_zone",
  "operator": "contains",
  "values": [{"value": "America"}]
}
```

### Metro Area Attributes

#### `geo.metro_code`
- **Description**: Metropolitan area code (primarily US)
- **Format**: Numeric code as string
- **Examples**: `"807"` (San Francisco), `"501"` (New York), `"803"` (Los Angeles)
- **Use Cases**: Metro area targeting, TV market segments, regional advertising

```json
{
  "attribute": "geo.metro_code",
  "operator": "in",
  "values": [{"value": "807"}, {"value": "501"}]
}
```

## Device Detection Attributes

Usertune also provides device and platform detection through geographic attributes:

### Device Type Attributes

#### `geo.is_mobile`
- **Description**: Whether the request comes from a mobile device
- **Format**: `"true"` or `"false"` as string
- **Use Cases**: Mobile-specific content, responsive design decisions

```json
{
  "attribute": "geo.is_mobile",
  "operator": "is_true",
  "values": []
}
```

#### `geo.is_tablet`
- **Description**: Whether the request comes from a tablet device
- **Format**: `"true"` or `"false"` as string
- **Use Cases**: Tablet-optimized content, app download prompts

```json
{
  "attribute": "geo.is_tablet",
  "operator": "is_true",
  "values": []
}
```

#### `geo.is_desktop`
- **Description**: Whether the request comes from a desktop device
- **Format**: `"true"` or `"false"` as string
- **Use Cases**: Desktop-specific features, larger screen layouts

```json
{
  "attribute": "geo.is_desktop",
  "operator": "is_true",
  "values": []
}
```

#### `geo.is_smart_tv`
- **Description**: Whether the request comes from a smart TV
- **Format**: `"true"` or `"false"` as string
- **Use Cases**: TV-optimized interfaces, streaming content

```json
{
  "attribute": "geo.is_smart_tv",
  "operator": "is_true",
  "values": []
}
```

### Platform Attributes

#### `geo.is_ios`
- **Description**: Whether the request comes from an iOS device
- **Format**: `"true"` or `"false"` as string
- **Use Cases**: iOS app store links, platform-specific content

```json
{
  "attribute": "geo.is_ios",
  "operator": "is_true",
  "values": []
}
```

#### `geo.is_android`
- **Description**: Whether the request comes from an Android device
- **Format**: `"true"` or `"false"` as string
- **Use Cases**: Google Play store links, Android-specific features

```json
{
  "attribute": "geo.is_android",
  "operator": "is_true",
  "values": []
}
```

### Language Attributes

#### `geo.language`
- **Description**: User's preferred language from Accept-Language header
- **Format**: Language code (RFC 5646)
- **Examples**: `"en-US"`, `"de-DE"`, `"ja-JP"`, `"fr-FR"`
- **Use Cases**: Language-specific content, localization

```json
{
  "attribute": "geo.language",
  "operator": "starts_with",
  "values": [{"value": "en"}]
}
```

### Technical Attributes

#### `geo.forwarded_proto`
- **Description**: Protocol used for the request
- **Format**: Protocol string
- **Examples**: `"https"`, `"http"`
- **Use Cases**: Security-based content, protocol-specific handling

```json
{
  "attribute": "geo.forwarded_proto",
  "operator": "equals",
  "values": [{"value": "https"}]
}
```

#### `geo.http_version`
- **Description**: HTTP version used for the request
- **Format**: HTTP version string
- **Examples**: `"1.1"`, `"2.0"`
- **Use Cases**: Performance optimization, feature availability

```json
{
  "attribute": "geo.http_version",
  "operator": "equals",
  "values": [{"value": "2.0"}]
}
```

## Usage Examples

### Geographic Targeting

#### Country-Specific Content
```json
// Condition for German users
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.country",
        "operator": "equals",
        "values": [{"value": "DE"}]
      }
    ]
  }
]

// Payload with German content
{
  "welcome": "Willkommen in Deutschland!",
  "currency": "EUR",
  "language": "de"
}
```

#### Regional Targeting
```json
// Condition for US West Coast
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.country",
        "operator": "equals",
        "values": [{"value": "US"}]
      },
      {
        "attribute": "geo.region",
        "operator": "in",
        "values": [{"value": "CA"}, {"value": "WA"}, {"value": "OR"}]
      }
    ]
  }
]
```

#### City-Specific Targeting
```json
// Condition for major cities
[
  {
    "logic": "or",
    "rules": [
      {
        "attribute": "geo.city",
        "operator": "in",
        "values": [
          {"value": "New York"},
          {"value": "Los Angeles"},
          {"value": "Chicago"}
        ]
      }
    ]
  }
]
```

### Device-Based Personalization

#### Mobile-First Content
```json
// Condition for mobile users
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.is_mobile",
        "operator": "is_true",
        "values": []
      }
    ]
  }
]

// Mobile-optimized payload
{
  "headline": "Mobile App Available",
  "cta": "Download Now",
  "layout": "mobile-optimized"
}
```

#### Platform-Specific App Promotion
```json
// iOS users
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.is_ios",
        "operator": "is_true",
        "values": []
      }
    ]
  }
]

// iOS-specific payload
{
  "app_store_link": "https://apps.apple.com/app/your-app",
  "cta": "Download on App Store",
  "badge": "app-store-badge.png"
}
```

### Combined Targeting

#### Location + Device Targeting
```json
// German mobile users
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.country",
        "operator": "equals",
        "values": [{"value": "DE"}]
      },
      {
        "attribute": "geo.is_mobile",
        "operator": "is_true",
        "values": []
      }
    ]
  }
]
```

#### Time Zone + Language Targeting
```json
// European users with English language preference
[
  {
    "logic": "and",
    "rules": [
      {
        "attribute": "geo.time_zone",
        "operator": "starts_with",
        "values": [{"value": "Europe"}]
      },
      {
        "attribute": "geo.language",
        "operator": "starts_with",
        "values": [{"value": "en"}]
      }
    ]
  }
]
```

## Dynamic Content with Geographic Variables

### Location-Based Messaging
```json
{
  "welcome": "Welcome from {geo.city}, {geo.country_name}!",
  "local_time": "Your time zone: {geo.time_zone}",
  "coordinates": "Location: {geo.latitude}, {geo.longitude}"
}
```

### Device-Aware Content
```json
{
  "device_message": "You're browsing from a {geo.is_mobile === 'true' ? 'mobile' : 'desktop'} device",
  "platform_specific": "Optimized for {geo.is_ios === 'true' ? 'iOS' : geo.is_android === 'true' ? 'Android' : 'web'}"
}
```

## Best Practices

### 1. Fallback Handling
Always plan for missing or empty geo data:
```json
{
  "location_message": "Hello from {geo.city || 'your location'}!"
}
```

### 2. Data Validation
Validate geo data before using in conditions:
```json
// Check if country exists and is not empty
{
  "attribute": "geo.country",
  "operator": "exists",
  "values": []
}
```

### 3. Privacy Considerations
- Be transparent about location-based personalization
- Respect user privacy preferences
- Consider GDPR/CCPA compliance for location data usage

### 4. Testing Recommendations
Test with various geographic locations:
- Different countries and regions
- Urban vs rural locations  
- Different device types and platforms
- Various time zones

## Limitations and Notes

### Data Accuracy
- Geographic data is based on IP geolocation and may not be 100% accurate
- VPN usage can affect location detection
- Mobile users may have less precise location data

### Device Detection
- Device detection is based on User-Agent headers
- Some devices may not be classified correctly
- Custom browsers or modified User-Agents may affect detection

### CloudFront Dependency
- Geographic data is provided by CloudFront headers
- Attributes may be empty if CloudFront is not used or configured
- Direct API access may not populate all geo attributes

---

Geographic attributes provide powerful targeting capabilities for creating location-aware and device-optimized experiences. Use them strategically to deliver relevant, personalized content to your users based on their location and device characteristics. 