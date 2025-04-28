# API Overview

The AsociaBet API allows you to integrate social media data from Twitter and YouTube into your applications. This documentation provides everything you need to implement our API quickly and efficiently.

## Base URL

All API requests should be made to the following base URL:

```
https://dev-api.asocia.bet/api
```

## Request Format

All requests should include:

- Your API key in the `X-API-Key` header
- For public keys, origin validation headers (`Origin` and `Referer`)
- Content-Type: `application/json` for POST requests

## Authentication

AsociaBet API uses API keys for authentication. We offer two types of keys:

1. **Private Keys** (`sk_*`) - For server-side applications
2. **Public Keys** (`pk_*`) - For client-side applications (with origin validation)

[Learn more about authentication →](authentication.md)

## Credit System

Our API uses a credit-based system for access control:

- Each request consumes 1 credit from your account
- Monitor your usage in your AsociaBet dashboard
- Purchase additional credits as needed for your application

## Response Format

All API responses are in JSON format with a consistent structure:

**Success Response**

```json
{
    "username": "example_user",
    "details": {
        // Data payload
    }
}
```

**Error Response**

```json
{
    "error": "Error message"
}
```

## Status Codes

The API uses standard HTTP status codes:

- `200` - Success
- `401` - Unauthorized (invalid API key)
- `402` - Payment Required (insufficient credits)
- `404` - Not Found
- `500` - Server Error

## Available Endpoints

### Twitter Data

Access Twitter user information and content:

- [User Details](endpoints/twitter.md#get-user-details) - Profile information
- [User Followers](endpoints/twitter.md#get-user-followers) - Follower data
- [User Tweets](endpoints/twitter.md#get-user-tweets) - User's tweet history
- [Single Tweet](endpoints/twitter.md#get-single-tweet) - Specific tweet details

### YouTube Data

Access YouTube channel information and content:

- [Channel Details](endpoints/youtube.md#get-channel-details) - Channel information
- [Channel Shorts](endpoints/youtube.md#get-channel-shorts) - Shorts video data
- [Single Short](endpoints/youtube.md#get-single-short) - Specific short details