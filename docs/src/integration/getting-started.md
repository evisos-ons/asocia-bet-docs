# Getting Started with the AsociaBet API

This guide will help you quickly integrate the AsociaBet API into your application to access Twitter and YouTube data.

## Step 1a: Create an Account

Before you can use the API, you'll need to:

1. Visit [dev.asocia.bet/signup](https://dev.asocia.bet/signup)
2. Login with your Gmail account

## Step 1b: Purchase credits

For the time being, contact the admin for test credits.

## Step 2: Generate Your API Keys

From your dashboard:

1. Navigate to "API Keys" section
2. Click "Generate New API Key"
3. Choose the key type:
   - **Private Key** (`sk_*`) - For server-side applications
   - **Public Key** (`pk_*`) - For client-side applications
4. For public keys, add your application's domains to the allowed origins list
5. Store your API key securely

## Step 3: Make Your First API Request

Let's verify everything works by making a simple request to list available Twitter creators:

### Using JavaScript

```javascript
// Check if your API key works
const checkApiAccess = async () => {
  try {
    const response = await fetch('https://dev-api.asocia.bet/api/twitter/creators', {
      headers: {
        'X-API-Key': 'your_api_key'
      }
    });
    
    const data = await response.json();
    console.log('Available creators:', data.creators);
    return data;
  } catch (error) {
    console.error('API request failed:', error);
  }
};

checkApiAccess();
```

### Using Python

```python
import requests

def check_api_access():
    headers = {
        'X-API-Key': 'your_api_key'
    }
    
    response = requests.get(
        'https://dev-api.asocia.bet/api/twitter/creators',
        headers=headers
    )
    
    if response.status_code == 200:
        print('Available creators:', response.json()['creators'])
        return response.json()
    else:
        print('Error:', response.json().get('error', 'Unknown error'))
        return None

check_api_access()
```

## Step 4: Integrate into Your Application

Now that you've confirmed your API key works, it's time to integrate the API into your application.

### Create an API Service

Build a reusable service to handle API requests:

```javascript
// apiService.js
class AsociaBetApi {
  constructor(apiKey, baseUrl = 'https://dev-api.asocia.bet/api') {
    this.apiKey = apiKey;
    this.baseUrl = baseUrl;
  }
  
  async request(endpoint, options = {}) {
    const url = `${this.baseUrl}${endpoint}`;
    
    const headers = {
      'X-API-Key': this.apiKey,
      ...options.headers
    };
    
    try {
      const response = await fetch(url, {
        ...options,
        headers
      });
      
      const data = await response.json();
      
      if (!response.ok) {
        throw new Error(data.error || 'API request failed');
      }
      
      return data;
    } catch (error) {
      console.error(`Error fetching ${endpoint}:`, error);
      throw error;
    }
  }
  
  // Twitter endpoints
  getTwitterCreators() {
    return this.request('/twitter/creators');
  }
  
  getTwitterUserDetails(username) {
    return this.request(`/twitter/${username}/details`);
  }
  
  getUserTweets(username) {
    return this.request(`/twitter/${username}/tweets`);
  }
  
  // YouTube endpoints
  getYoutubeChannels() {
    return this.request('/youtube/channels');
  }
  
  getChannelDetails(channel) {
    return this.request(`/youtube/${channel}/details`);
  }
  
  getChannelShorts(channel) {
    return this.request(`/youtube/${channel}/shorts`);
  }
}

// Usage
const api = new AsociaBetApi('your_api_key');
api.getTwitterUserDetails('elonmusk')
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

## Step 5: Implement Error Handling

Always implement proper error handling for API requests:

```javascript
async function fetchTwitterData(username) {
  try {
    const response = await api.getTwitterUserDetails(username);
    updateUI(response);
  } catch (error) {
    if (error.message.includes('not found')) {
      showNotFoundError();
    } else if (error.message.includes('insufficient credits')) {
      showCreditsPurchasePrompt();
    } else if (error.message.includes('invalid API key')) {
      showInvalidKeyError();
    } else {
      showGenericError();
    }
  }
}
```

## Step 6: Monitor Usage

Keep track of your API usage:

1. Check your dashboard regularly for credit usage
2. Set up alerts for low credit balance
3. Monitor response times and errors

## Next Steps

- Explore [Twitter API Endpoints](../api/endpoints/twitter.md)
- Explore [YouTube API Endpoints](../api/endpoints/youtube.md)
- Check out [Client Libraries](client-libraries.md) for easier integration
- Review [Integration Best Practices](best-practices.md)