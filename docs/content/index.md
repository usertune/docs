# Content

## What is Content?

**Content** is a container that holds multiple variants and manages how they are delivered to users. Content items don't store the actual content themselves, but rather act as orchestrators that determine which variant to serve based on client location, user information, and other contextual factors.

## Key Concepts

- **Variant Container**: Content holds multiple variants, each with different templates or approaches
- **Dynamic Selection**: Automatically chooses which variant to serve based on context
- **Traffic Management**: Controls how users are distributed across different variants
- **Context-Aware**: Uses location, time, user attributes, and custom data to make decisions
- **A/B Testing Hub**: Enables experimentation by managing variant traffic distribution
- **Visibility Control**: Use the `is_published` toggle to control whether content is accessible

## Content Architecture

### Content as Orchestrator
Content items serve as intelligent containers that:
- **Hold Variants**: Store multiple versions of your content
- **Route Traffic**: Decide which variant each user sees
- **Apply Context**: Use geographic, temporal, and user data for decisions
- **Manage Experiments**: Control A/B testing and traffic splitting
- **Control Access**: Determine whether the entire content system is available

### Content vs Variants
- **Content**: The container and traffic management logic
- **Variants**: The actual templates and content that users see
- **Relationship**: One Content item can have multiple Variants

## Content Properties

Every content item has these management attributes:

- **Name**: Display name for identifying the content container
- **Slug**: URL-friendly identifier used in API endpoints  
- **Description**: Optional explanation of the content's purpose
- **is_published**: Boolean toggle that controls content visibility and accessibility
- **Variant Management**: Configuration for how variants are selected and served

## Content Visibility Control

### is_published Toggle

The `is_published` field is a crucial control that determines whether your content container is accessible:

- **Published (true)**: Content is active and can serve variants to users
- **Unpublished (false)**: Content is hidden and will return "not found" responses

### Visibility Matrix

| Content Published | Workspace Type | API Result |
|------------------|----------------|------------|
| **Yes** | Private | Accessible with authentication |
| **Yes** | Public | Accessible without authentication |
| **No** | Private | Not accessible (404 error) |
| **No** | Public | Not accessible (404 error) |

### Use Cases for Unpublishing

- **Development**: Hide content while building and testing variants
- **Maintenance**: Temporarily disable content during updates
- **Scheduled Releases**: Prepare content before launch
- **Emergency Shutdown**: Quickly disable problematic content
- **Seasonal Content**: Activate/deactivate content based on timing

## How Content Works

### 1. Access Control
Before processing any request:
1. Check if content `is_published` is true
2. If unpublished, return 404 "Content not found"
3. If published, proceed to variant selection

### 2. Request Processing
When a user requests published content:
1. Content receives the API request with context data
2. Analyzes user location, time, and attributes
3. Determines which variant to serve based on rules
4. Returns the selected variant's rendered template

### 3. Variant Selection
Content uses various factors to choose variants:
- **Geographic Rules**: Serve different variants by location
- **Time-Based Logic**: Change content based on time of day/season
- **User Attributes**: Personalize based on user characteristics
- **Traffic Splitting**: A/B test by distributing users across variants

### 4. Context Integration
Content leverages contextual data:
- **Geographic Data**: Country, city, region information
- **Time Information**: Current time, date, season
- **User Attributes**: Custom user properties and behaviors
- **Custom Context**: Any additional data passed via API

## Common Use Cases

### A/B Testing Management
- **Campaign Testing**: Test different marketing messages
- **UI Optimization**: Compare different interface elements
- **Content Strategy**: Experiment with various content approaches
- **Performance Optimization**: Find the most effective variants

### Geographic Personalization
- **Location-Based Content**: Serve region-specific information
- **Language Localization**: Route to appropriate language variants
- **Cultural Adaptation**: Customize content for different markets
- **Compliance**: Ensure content meets local regulations

### User Segmentation
- **Experience Level**: Different content for new vs returning users
- **Demographics**: Age, preferences, or behavior-based variants
- **Device Optimization**: Mobile vs desktop optimized variants
- **Access Level**: Free vs premium user experiences

## API Endpoints

Content is accessed through workspace APIs:

```
GET /v1/workspace/{workspace_id}/content/{content_slug}
```

The content system first checks `is_published` status, then processes the request and returns the appropriate variant based on context.

## Getting Started

Ready to create your first content container? Follow our [Getting Started guide](getting-started.md) to learn how to set up content items and add variants for testing and personalization.

## Advanced Features

### Public Content
Learn about [Public Content](public-content.md) to understand how content containers work in public workspaces and manage variant selection without authentication.

### Variant Management
Content items manage their variants for A/B testing and personalization. See the [Variant documentation](../variant/index.md) to learn about creating and managing the actual content templates.

---

Content items serve as intelligent containers within workspaces, managing how variants are selected and delivered based on user context, location, and behavior patterns. 