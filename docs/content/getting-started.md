# Getting Started with Content

Follow these steps to create your first content container in Usertune:

## Prerequisites

Before creating content, you need:
- A Usertune account (sign up at [app.usertune.io](https://app.usertune.io))
- At least one workspace created (see [Workspace Getting Started](../workspace/getting-started.md))

## Step 1: Navigate to Your Workspace

1. Sign in to [app.usertune.io](https://app.usertune.io)
2. Go to [app.usertune.io/workspaces](https://app.usertune.io/workspaces)
3. Click on the workspace where you want to create content
4. You'll see the workspace dashboard with existing content containers (if any)

## Step 2: Create New Content Container

1. Click the **"New Content"** or **"Add Content"** button
2. A content creation form will appear with the following fields:

### Required Information:
- **Name**: Enter a descriptive name for your content container
  - Example: "Homepage Hero", "Product Recommendation", "Welcome Message"
  - Choose names that clearly identify the container's purpose

- **Slug**: URL-friendly identifier for API access
  - Automatically generated from the name
  - Example: "homepage-hero", "product-recommendation"
  - Used in API endpoints: `/v1/workspace/{workspace_id}/content/{slug}`

### Container Configuration:
- **Description**: Detailed explanation of what this content container manages
- **is_published**: Toggle to control whether the container is accessible via API
- **Tags**: Organize content containers with searchable tags

## Step 3: Configure Content Container

### Basic Container Setup:
```
Name: "Homepage Hero"
Slug: "homepage-hero" (auto-generated)
Description: "Container for homepage hero variants with A/B testing"
Published: Yes (enabled)
```

### Advanced Container Setup:
```
Name: "Product Recommendations"
Slug: "product-recommendations"  
Description: "Manages recommendation variants based on user location and behavior"
Published: Yes (enabled)
Tags: ["recommendations", "personalization", "products"]
```

## Step 4: Understanding Container Purpose

### What the Content Container Does:
- **Holds Variants**: Acts as a parent container for multiple content variants
- **Routes Requests**: Determines which variant to serve to each user
- **Manages Traffic**: Controls how users are distributed across variants
- **Applies Context**: Uses location, time, and user data for variant selection

### What the Content Container Does NOT Do:
- **Store Templates**: Content containers don't hold actual templates or content
- **Render Content**: Containers don't generate or process content themselves
- **Store Data**: No actual content data is stored in the container

## Step 5: Create Your First Variant

After creating the content container, you need to add variants:

1. Click **"Add Variant"** or **"Create Variant"** within your content container
2. Variants contain the actual templates and content that users will see
3. You can create multiple variants for A/B testing

**Note**: A content container without variants cannot serve any content to users.

## Step 6: Publish Content Container

1. Review your container configuration
2. Ensure the **"Published"** toggle is enabled
3. Click **"Save"** or **"Create"** to finalize
4. Your content container is now ready to manage variants

## API Usage

Content containers are accessed via API, but they route to variants:

```
GET /v1/workspace/{workspace_id}/content/{content_slug}
```

### What Happens During API Call:
1. **Container Check**: Verify content container exists and is published
2. **Variant Selection**: Container logic determines which variant to serve
3. **Context Processing**: Apply user location, time, and attribute data
4. **Variant Response**: Return the selected variant's rendered content

## Next Steps

After creating your content container:

1. **Add Variants**: Create the actual content templates within your container
2. **Configure Traffic**: Set up how users are distributed across variants
3. **Test Selection**: Verify the container routes to correct variants
4. **Monitor Performance**: Track how different variants perform

## Content Container Management

### Container Organization
- **Naming**: Use clear, descriptive names for easy identification
- **Grouping**: Use tags to organize related containers
- **Documentation**: Maintain descriptions explaining container purpose

### Access Control
- **Publishing**: Use `is_published` to control container availability
- **Workspace Level**: Container inherits workspace public/private settings
- **Variant Dependencies**: Containers need variants to serve content

## Common Container Types

### A/B Testing Containers
- **Purpose**: Manage experiments between different content approaches
- **Variants**: Multiple versions for comparison
- **Traffic**: Split users across variants for testing

### Geographic Content Containers
- **Purpose**: Serve location-appropriate content
- **Variants**: Different content for different regions
- **Selection**: Route based on user geographic data

### User Segment Containers
- **Purpose**: Personalize based on user characteristics
- **Variants**: Content optimized for different user types
- **Logic**: Select variants based on user attributes

---

Ready to add actual content? Continue to [Variants](../variant/getting-started.md) to learn how to create the templates and content that your container will manage. 