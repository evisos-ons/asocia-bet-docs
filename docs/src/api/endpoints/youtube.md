# YouTube API Endpoints

The YouTube API endpoints allow you to access YouTube channel and shorts data for integration into your applications. Here you'll find all available endpoints, request parameters, and response formats.

## Base URL

All YouTube API endpoints use the following base URL:

```
https://dev-api.asocia.bet/api/youtube
```

## Available Endpoints

### Get Available Channels

Retrieve a list of all available YouTube channels.

```http
GET /channels
```

**Code Example**

```javascript
// JavaScript example
fetch('https://dev-api.asocia.bet/api/youtube/channels', {
  headers: {
    'X-API-Key': 'your_api_key'
  }
})
.then(response => response.json())
.then(data => console.log(data));
```

**Response**

```json
{
  "channels": [
    {
      "id": "UC-lHJZR3Gqxm24_Vd_AJ5Yw",
      "username": "PewDiePie",
      "displayName": "PewDiePie",
      "subscribers": 111000000,
      "videoCount": 4500,
      "thumbnail": "https://yt3.ggpht.com/...",
    },
    {
      "id": "UCX6OQ3DkcsbYNE6H8uQQuVA",
      "username": "MrBeast",
      "displayName": "MrBeast",
      "subscribers": 158000000,
      "videoCount": 750,
      "thumbnail": "https://yt3.ggpht.com/...",
    }
  ]
}
```

### Get Channel Details

Retrieve detailed information about a YouTube channel.

```http
GET /{channel}/details
```

**Parameters**

| Parameter | Type   | Required | Description          |
|-----------|--------|----------|----------------------|
| channel   | string | Yes      | YouTube channel username or ID |

**Code Example**

```javascript
// JavaScript example
const getChannelDetails = async (channel) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/youtube/${channel}/details`, {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('Error fetching channel details:', error);
  }
};
```

**Response**

```json
{
  "id": "UCX6OQ3DkcsbYNE6H8uQQuVA",
  "username": "MrBeast",
  "displayName": "MrBeast",
  "description": "Gaming, vlogs, and more!",
  "subscribers": 158000000,
  "videoCount": 750,
  "viewCount": 25000000000,
  "joinedAt": "2012-02-19T20:05:05Z",
  "country": "US",
  "thumbnail": "https://yt3.ggpht.com/...",
  "bannerImage": "https://yt3.ggpht.com/...",
  "statistics": {
    "dailySubscriberGain": 50000,
    "weeklySubscriberGain": 350000,
    "monthlySubscriberGain": 1500000
  }
}
```

### Get Channel Shorts

Retrieve a list of shorts from a YouTube channel.

```http
GET /{channel}/shorts
```

**Parameters**

| Parameter | Type   | Required | Description                   |
|-----------|--------|----------|-------------------------------|
| channel   | string | Yes      | YouTube channel username or ID |
| limit     | number | No       | Maximum number of shorts to return (default: 20, max: 100) |
| before    | string | No       | Return shorts published before this date (ISO 8601 format) |

**Code Example**

```javascript
// JavaScript example
const getChannelShorts = async (channel) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/youtube/${channel}/shorts`, {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('Error fetching channel shorts:', error);
  }
};
```

**Response**

```json
{
  "channel": "MrBeast",
  "shorts": [
    {
      "id": "abc123xyz",
      "title": "I Gave Away 100 Cars!",
      "thumbnail": "https://i.ytimg.com/vi/...",
      "publishedAt": "2024-03-01T15:00:00Z",
      "views": 50000000,
      "likes": 2500000,
      "comments": 100000
    }
  ],
  "pagination": {
    "hasMore": true,
    "nextCursor": "eyJwYWdlIjoy..."
  }
}
```

### Get Short Details

Retrieve detailed metrics for a specific YouTube short.

```http
GET /{channel}/short/{short_id}
```

**Parameters**

| Parameter | Type   | Required | Description                   |
|-----------|--------|----------|-------------------------------|
| channel   | string | Yes      | YouTube channel username or ID |
| short_id  | string | Yes      | Unique identifier of the short |

**Code Example**

```javascript
// JavaScript example
const getShortDetails = async (channel, shortId) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/youtube/${channel}/short/${shortId}`, {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('Error fetching short details:', error);
  }
};
```

**Response**

```json
{
  "id": "abc123xyz",
  "title": "I Gave Away 100 Cars!",
  "description": "Watch till the end to see who won!",
  "publishedAt": "2024-03-01T15:00:00Z",
  "metrics": {
    "views": 50000000,
    "likes": 2500000,
    "comments": 100000,
    "shares": 75000
  },
  "history": [
    {
      "timestamp": "2024-03-01T16:00:00Z",
      "views": 1000000,
      "likes": 50000,
      "comments": 2000
    }
  ]
}
```

## Error Handling

### Channel Not Found Error

```json
{
    "error": "Channel channel not found"
}
```

### Short Not Found Error

```json
{
    "error": "Short short_id not found for channel channel"
}
```

## Credits & Rate Limiting

Each YouTube API request costs 1 credit. Ensure your account has sufficient credits before making requests.

## Next Steps

- [Twitter API Endpoints](twitter.md)
- [Authentication Guide](../authentication.md)
- [Integration Examples](../../integration/getting-started.md)