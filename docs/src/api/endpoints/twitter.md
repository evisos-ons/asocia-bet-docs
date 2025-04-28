# Twitter API Endpoints

The Twitter API endpoints allow you to access Twitter user data for integration into your applications. Here you'll find all available endpoints, request parameters, and response formats.

## Base URL

All Twitter API endpoints use the following base URL:

```
https://dev-api.asocia.bet/api/twitter
```

## Available Endpoints

### Get User Details

Retrieve detailed information about a Twitter user.

```http
GET /{username}/details
```

**Parameters**

| Parameter | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| username  | string | Yes      | Twitter username without @ symbol  |

**Code Example**

```javascript
// JavaScript example
const getUserDetails = async (username) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/twitter/${username}/details`, {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('Error fetching Twitter user details:', error);
  }
};
```

**Response**

```json
{
  "username": "elonmusk",
  "displayName": "Elon Musk",
  "followers_count": 158900000,
  "following_count": 1500,
  "tweet_count": 25000,
  "profile_image_url": "https://pbs.twimg.com/profile_images/123456789/elon_400x400.jpg",
  "description": "Technoking of Tesla, CEO of SpaceX",
  "created_at": "2009-06-02T20:12:29.000Z",
  "verified": true
}
```

### Get User Tweets

Retrieve the latest tweets for a Twitter user.

```http
GET /{username}/tweets
```

**Parameters**

| Parameter | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| username  | string | Yes      | Twitter username without @ symbol  |

**Code Example**

```javascript
// JavaScript example
const getUserTweets = async (username) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/twitter/${username}/tweets`, {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('Error fetching tweets:', error);
  }
};
```

**Response**

```json
{
  "tweets": {
    "1449099963531608064": {
      "created_at": "2021-10-15T19:48:48.000Z",
      "metrics": [
        {
          "likes": 78,
          "replies": 6,
          "retweets": 10,
          "timestamp": "2025-04-23T17:01:08.390353",
          "views": 0
        }
      ],
      "text": "@aeger08 @BabyApesNFT WAWAWA!! \nmissed mint, sniped perfect two! https://t.co/jD3FaToVzz",
      "timestamp": "2021-10-15T19:48:48.000Z",
      "url": "https://x.com/chanchaneeNFT/status/1449099963531608064"
    },
    "1513470800132972544": {
      "created_at": "2022-04-11T10:55:32.000Z",
      "metrics": [
        {
          "likes": 50,
          "replies": 1,
          "retweets": 8,
          "timestamp": "2025-04-23T17:01:08.390769",
          "views": 0
        }
      ],
      "text": "@TheMarketComeUp @CetsOnCreck @DegenDojoNFT lfg! https://t.co/P9gxWtPGkl",
      "timestamp": "2022-04-11T10:55:32.000Z",
      "url": "https://x.com/chanchaneeNFT/status/1513470800132972544"
    }
  }
}
```

### Get User Followers

Retrieve the follower count and follower growth metrics for a Twitter user.

```http
GET /{username}/followers
```

**Parameters**

| Parameter | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| username  | string | Yes      | Twitter username without @ symbol  |

**Code Example**

```javascript
// JavaScript example
const getUserFollowers = async (username) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/twitter/${username}/followers`, {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('Error fetching followers:', error);
  }
};
```

**Response**

```json
{
  "username": "elonmusk",
  "current_followers": 158900000,
  "history": [
    {
      "date": "2024-03-01",
      "followers": 158800000,
      "change": 100000
    },
    {
      "date": "2024-02-29",
      "followers": 158700000,
      "change": 50000
    }
  ]
}
```

### Get Tweet Details

Retrieve detailed metrics for a specific tweet.

```http
GET /{username}/tweet/{tweet_id}
```

**Parameters**

| Parameter | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| username  | string | Yes      | Twitter username without @ symbol  |
| tweet_id  | string | Yes      | Unique identifier of the tweet     |

**Code Example**

```javascript
// JavaScript example
const getTweetDetails = async (username, tweetId) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/twitter/${username}/tweet/${tweetId}`, {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('Error fetching tweet details:', error);
  }
};
```

**Response**

```json
{
  "id": "1234567890",
  "text": "To the moon! 🚀",
  "created_at": "2024-03-01T12:00:00.000Z",
  "metrics": {
    "impressions": 1500000,
    "likes": 50000,
    "retweets": 10000,
    "replies": 5000,
    "quotes": 2000
  },
  "history": [
    {
      "timestamp": "2024-03-01T13:00:00.000Z",
      "impressions": 1000000,
      "likes": 30000,
      "retweets": 8000
    }
  ]
}
```

## Error Handling

### User Not Found Error

```json
{
    "error": "User username not found"
}
```

### Tweet Not Found Error

```json
{
    "error": "Tweet tweet_id not found for user username"
}
```

## Credits & Rate Limiting

Each Twitter API request costs 1 credit. Ensure your account has sufficient credits before making requests.

## Next Steps

- [YouTube API Endpoints](youtube.md)
- [Authentication Guide](../authentication.md)
- [Integration Examples](../../integration/getting-started.md)