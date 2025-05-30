# Public Content

## Overview

**Public Content** refers to content items that exist within public workspaces and can be accessed without authentication. When a workspace is marked as public, all content within that workspace becomes publicly accessible via API endpoints.

## What Makes Content Public?

Content becomes public automatically when:
1. The **workspace** containing the content has `is_public` set to `true`
2. The **content item** has `is_published` set to `true`

Both conditions must be met for content to be publicly accessible.

## Content Visibility Matrix

| Workspace Type | Content Published | Result |
|---------------|------------------|---------|
| Private | Yes | Private (auth required) |
| Private | No | Private (not accessible) |
| Public | Yes | **Public** (no auth required) |
| Public | No | Not accessible |

## How Public Content Works

### Accessibility
Public content can be accessed by anyone with the proper API endpoint, without requiring authentication tokens or user permissions.

### API Endpoints
Public content uses the same API endpoints as private content:

```
# Content in public workspace (no auth required)
GET /v1/workspace/{workspace_id}/content/{content_slug}

# Content in private workspace (auth required)  
GET /v1/workspace/{workspace_id}/content/{content_slug}
Authorization: Bearer {token}
```

The difference is that public workspace content doesn't require the `Authorization` header.

### Personalization Features
Public content retains full personalization capabilities:

- **Context Variables**: Geographic data, time information, custom attributes
- **Template Logic**: Conditional content based on user context
- **Dynamic Rendering**: Real-time content generation
- **Variant Testing**: A/B testing still functions normally

## Content Management for Public Workspaces

### Creating Public-Ready Content

When creating content for public workspaces:

1. **Avoid Sensitive Information**: Never include private data, API keys, or confidential information
2. **Plan for Public Access**: Design content assuming anyone can view the template
3. **Use Generic Variables**: Focus on geo, time, and passed attributes rather than user-specific data
4. **Test Thoroughly**: Verify content works without authentication

### Content Security Considerations

#### What Remains Protected:
- **Content Settings**: Creation date, author, internal metadata
- **Analytics**: Performance metrics and usage statistics
- **Workspace Configuration**: Workspace settings and permissions
- **Edit Access**: Only workspace owners can modify content

#### What Becomes Visible:
- **Template Source**: The actual Twig template code
- **Variable Structure**: Available personalization variables
- **Logic Flow**: Conditional statements and personalization rules
- **Output**: The rendered content for any context

### Best Practices

1. **Content Review**: Regularly audit public content for appropriateness
2. **Template Security**: Avoid exposing internal logic or system information
3. **Variable Validation**: Ensure templates handle missing or invalid data gracefully
4. **Performance**: Optimize templates for public access patterns
5. **Monitoring**: Track usage patterns and performance metrics

## Publishing Workflow

### Making Content Public

1. **Create Content**: Develop your content in a workspace
2. **Test Privately**: Verify functionality while workspace is private
3. **Review Template**: Audit template for public readiness
4. **Make Workspace Public**: Enable public access on the workspace
5. **Monitor Access**: Track public usage and performance

### Converting Private to Public

When moving content from private to public workspace:

1. **Content Audit**: Review all templates for sensitive information
2. **Variable Cleanup**: Remove user-specific or private variables
3. **Access Testing**: Verify content works without authentication
4. **Performance Check**: Ensure public access doesn't impact performance
5. **Documentation**: Update team documentation about public status

## Monitoring Public Content

### Key Metrics
- **Access Volume**: Number of requests to public content
- **Geographic Distribution**: Where requests originate
- **Performance**: Response times for public endpoints
- **Error Rates**: Failed requests or template errors

### Usage Analytics
- **Popular Content**: Most frequently accessed public content
- **Peak Times**: When public content sees highest usage
- **Context Patterns**: Common personalization variable combinations
- **Conversion Tracking**: How public content affects user behavior

## Limitations and Considerations

### Performance Impact
- Public content may experience higher traffic volumes
- Consider caching strategies for frequently accessed content
- Monitor response times and optimize templates as needed

### Template Exposure
- Template logic becomes visible to anyone accessing the API
- Avoid revealing business logic or proprietary algorithms
- Keep templates focused on presentation rather than complex processing

### Version Control
- Public content changes are immediately visible to all users
- Consider staging processes for public content updates
- Plan rollback strategies for public content issues

---

For information about setting up public workspaces, see [Public Workspaces](../workspace/public-workspaces.md). To learn about A/B testing public content, continue to [Variants](../variant/index.md). 