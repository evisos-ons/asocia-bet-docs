# Authentication

The AsociaBet API uses API keys to authenticate requests. This guide explains how to obtain and implement API keys in your applications.

## Getting Your API Keys

1. **Create an account**: Sign up at [dev-api.asocia.bet/signup](https://dev-api.asocia.bet/signup)
2. **Visit the Dashboard**: Navigate to the API Keys section
3. **Generate Keys**: Create the type of key you need (private or public)
4. **Set Allowed Origins**: For public keys, specify the domains that can use this key

## API Key Types

### Private Keys (`sk_*`)

Use private keys for server-side applications. These provide full access but must be kept secure.

```javascript
// Node.js example with private key
const axios = require('axios');

const getTwitterUser = async (username) => {
  try {
    const response = await axios.get(`https://dev-api.asocia.bet/api/twitter/${username}/details`, {
      headers: {
        'X-API-Key': 'sk_your_private_key'
      }
    });
    return response.data;
  } catch (error) {
    console.error('API request failed:', error.response.data);
    return error.response.data;
  }
};
```

### Public Keys (`pk_*`)

Use public keys for client-side applications. These require origin validation and have limited permissions.

```javascript
// Browser example with public key
const getTwitterUser = async (username) => {
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api/twitter/${username}/details`, {
      headers: {
        'X-API-Key': 'pk_your_public_key'
      }
    });
    return response.json();
  } catch (error) {
    console.error('API request failed:', error);
    return { error: 'Failed to fetch data' };
  }
};
```

## Origin Validation (Public Keys)

When using public keys in browsers, we automatically validate:

1. The `Origin` header - Must match an allowed domain in your API key settings
2. The `Referer` header - Must be consistent with the Origin

### Common Origin Validation Errors

```json
{
    "error": "Origin header is required for public API keys"
}
```

```json
{
    "error": "Origin not allowed for this API key"
}
```

## Securing Your API Keys

Follow these best practices to protect your API keys:

1. **Never expose private keys**: Keep private keys on your server, never in client-side code
2. **Environment variables**: Store keys in environment variables, not in code
3. **Restrict origins**: Limit allowed domains for public keys
4. **Monitor usage**: Check your dashboard regularly for unusual activity
5. **Rotate keys**: Generate new keys periodically and phase out old ones

## Using API Keys With Popular Frameworks

### React

```javascript
// API service in React
import axios from 'axios';

const apiService = axios.create({
  baseURL: 'https://dev-api.asocia.bet/api',
  headers: {
    'X-API-Key': process.env.REACT_APP_ASOCIABET_API_KEY
  }
});

export const getTwitterUserDetails = (username) => {
  return apiService.get(`/twitter/${username}/details`);
};
```

### Python

```python
# Python example
import requests

def get_twitter_user(username):
    headers = {
        'X-API-Key': 'sk_your_private_key'
    }
    response = requests.get(
        f'https://dev-api.asocia.bet/api/twitter/{username}/details',
        headers=headers
    )
    return response.json()
```

## Next Steps

- [Explore API Endpoints](endpoints/twitter.md)
- [Integration Guide](../integration/getting-started.md)
- [Client Libraries](../integration/client-libraries.md)