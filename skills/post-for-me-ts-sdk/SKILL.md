---
name: post-for-me-ts-sdk
description: How to use the post-for-me Typescript SDK. For access to more information, try out the post-for-me MCP server.
---

# Post For Me TypeScript SDK Guide

## Overview

The Post For Me SDK enables cross-platform social media posting to Instagram, Facebook, TikTok, YouTube, Twitter/X, LinkedIn, Pinterest, Bluesky, and Threads. Create posts once and publish to multiple platforms simultaneously with platform-specific customizations.

## Installation

Install the Post For Me TypeScript SDK from npm:

```bash
npm install post-for-me
```

## Core Concepts

- **Social Accounts**: Connected platform accounts that can receive posts
- **Social Posts**: Content (caption + media) to be published
- **Post Results**: Outcome of publishing to each platform
- **Account Feeds**: Retrieve posts and metrics from connected accounts

## Creating Posts

### Basic Post (Instant Publishing)

```typescript
const post = await client.socialPosts.create({
  caption: "Check out our new product! #launch",
  social_accounts: ["sa_abc123", "sa_def456"],
  media: [{ url: "https://example.com/image.jpg" }],
});
```

### Scheduled Post

```typescript
const post = await client.socialPosts.create({
  caption: "Scheduled announcement",
  social_accounts: ["sa_abc123"],
  scheduled_at: "2024-12-31T18:00:00Z", // ISO 8601 format
});
```

### Draft Post

```typescript
const draft = await client.socialPosts.create({
  caption: "Work in progress",
  social_accounts: ["sa_abc123"],
  isDraft: true, // Won't be processed/published
});
```

## Platform-Specific Customization

### Instagram Reels

```typescript
await client.socialPosts.create({
  caption: "Check this reel!",
  social_accounts: ["sa_instagram"],
  media: [{ url: "https://example.com/video.mp4" }],
  platform_configurations: {
    instagram: {
      placement: "reels",
      share_to_feed: true, // Also show in feed
    },
  },
});
```

### Twitter/X with Poll

```typescript
await client.socialPosts.create({
  caption: "What feature should we build next?",
  social_accounts: ["sa_twitter"],
  platform_configurations: {
    x: {
      poll: {
        duration_minutes: 1440, // 24 hours
        options: ["Dark mode", "API v2", "Mobile app", "Analytics"],
      },
    },
  },
});
```

### TikTok Configuration

```typescript
await client.socialPosts.create({
  caption: "New dance challenge!",
  social_accounts: ["sa_tiktok"],
  media: [{ url: "https://example.com/dance.mp4" }],
  platform_configurations: {
    tiktok: {
      allow_comment: true,
      allow_duet: true,
      allow_stitch: true,
      privacy_status: "public",
    },
  },
});
```

## Managing Social Accounts

### List Accounts

```typescript
const accounts = await client.socialAccounts.list({
  platform: ["instagram", "tiktok"],
  status: ["connected"],
});
```

### Connect Account (OAuth Flow)

```typescript
// Step 1: Generate auth URL
const { url } = await client.socialAccounts.createAuthURL({
  platform: "instagram",
  external_id: "user_123",
});
// Redirect user to `url` to complete OAuth

// Step 2: After OAuth callback, account is auto-created
```

### Disconnect Account

```typescript
await client.socialAccounts.disconnect("sa_abc123");
```

## Media Upload

```typescript
// Request upload URL
const { media_url, upload_url } = await client.media.createUploadURL();

// Upload file to signed URL
await fetch(upload_url, {
  method: "PUT",
  headers: { "Content-Type": "video/mp4" },
  body: videoFile,
});

// Use media_url in post
await client.socialPosts.create({
  caption: "My video",
  social_accounts: ["sa_abc123"],
  media: [{ url: media_url }],
});
```

## Retrieving Post Results

```typescript
// Get results for a specific post
const results = await client.socialPostResults.list({
  post_id: ["sp_xyz789"],
});

results.data.forEach((result) => {
  console.log(`Platform: ${result.social_account_id}`);
  console.log(`Success: ${result.success}`);
  if (result.platform_data.url) {
    console.log(`Posted at: ${result.platform_data.url}`);
  }
});
```

## Fetching Account Feeds with Metrics

```typescript
const feed = await client.socialAccountFeeds.list("sa_instagram123", {
  expand: ["metrics"], // Include analytics
  limit: 20,
});

feed.data.forEach((post) => {
  console.log(`Caption: ${post.caption}`);
  if (post.metrics) {
    console.log(`Likes: ${post.metrics.likes}`);
    console.log(`Comments: ${post.metrics.comments}`);
  }
});
```

## Pagination

```typescript
const posts = await client.socialPosts.list({
  limit: 50,
  offset: 0,
  status: ["processed"],
});

console.log(`Total: ${posts.meta.total}`);
console.log(`Next page: ${posts.meta.next}`);
```
