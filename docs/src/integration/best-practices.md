# API Integration Best Practices

Follow these best practices to ensure optimal performance, security, and reliability when integrating with the AsociaBet API.

## Security

### API Key Management

- **Never expose private keys**: Keep private keys (`sk_*`) on your server-side code only
- **Environment variables**: Store API keys in environment variables, not in source code
- **Key rotation**: Implement a process to periodically rotate API keys
- **Separate development/production keys**: Use different API keys for different environments

```javascript
// GOOD - Using environment variables
const apiKey = process.env.ASOCIABET_API_KEY;

// BAD - Hardcoded keys
const apiKey = "sk_1234567890abcdef"; // Never do this!
```

### Public Key Usage

When using public keys (`pk_*`) in client-side applications:

- **Restrict domains**: Limit allowed origins to only the domains you own
- **Request validation**: Add additional validation on your server for requests from clients
- **Content Security Policy**: Implement a CSP to prevent unauthorized API calls

## Performance

### Caching

Implement appropriate caching strategies to reduce API calls:

```javascript
// Example caching implementation
class CachedApiClient {
  constructor(apiKey, cacheTtl = 300000) { // 5 minutes default TTL
    this.apiKey = apiKey;
    this.cacheTtl = cacheTtl;
    this.cache = new Map();
  }
  
  async request(endpoint) {
    const cacheKey = endpoint;
    
    // Check cache first
    if (this.cache.has(cacheKey)) {
      const cachedData = this.cache.get(cacheKey);
      if (Date.now() < cachedData.expiry) {
        return cachedData.data;
      }
    }
    
    // Make API request if not cached or expired
    const response = await fetch(`https://dev-api.asocia.bet/api${endpoint}`, {
      headers: {
        'X-API-Key': this.apiKey
      }
    });
    
    const data = await response.json();
    
    // Store in cache
    this.cache.set(cacheKey, {
      data,
      expiry: Date.now() + this.cacheTtl
    });
    
    return data;
  }
}
```

### Batching Requests

Whenever possible, batch related requests:

- Load user details and tweets in a single batch operation
- Implement client-side request queuing
- Schedule non-critical updates for off-peak times

### Pagination

For endpoints that return large datasets:

- Implement proper pagination in your application
- Only fetch the data you need
- Load additional data on demand

## Reliability

### Error Handling

Implement robust error handling for all API requests:

```javascript
async function fetchWithRetry(url, options, maxRetries = 3) {
  let retries = 0;
  
  while (retries < maxRetries) {
    try {
      const response = await fetch(url, options);
      
      if (response.status === 429) {
        // Rate limited, wait before retrying
        const retryAfter = response.headers.get('Retry-After') || 5;
        await new Promise(resolve => setTimeout(resolve, retryAfter * 1000));
        retries++;
        continue;
      }
      
      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.error || `HTTP error ${response.status}`);
      }
      
      return await response.json();
    } catch (error) {
      if (retries >= maxRetries - 1) {
        throw error;
      }
      
      // Exponential backoff
      const backoff = Math.pow(2, retries) * 1000;
      await new Promise(resolve => setTimeout(resolve, backoff));
      retries++;
    }
  }
}
```

### Rate Limit Handling

Be prepared to handle rate limits:

- Respect the `Retry-After` header
- Implement exponential backoff
- Monitor credit usage to avoid unexpected rate limiting

## Monitoring & Debugging

### Logging

Implement comprehensive logging for API interactions:

```javascript
async function loggedApiRequest(endpoint, apiKey) {
  const startTime = Date.now();
  
  try {
    const response = await fetch(`https://dev-api.asocia.bet/api${endpoint}`, {
      headers: {
        'X-API-Key': apiKey
      }
    });
    
    const data = await response.json();
    
    // Log successful request
    console.log({
      endpoint,
      status: response.status,
      duration: Date.now() - startTime,
      timestamp: new Date().toISOString()
    });
    
    return data;
  } catch (error) {
    // Log error
    console.error({
      endpoint,
      error: error.message,
      duration: Date.now() - startTime,
      timestamp: new Date().toISOString()
    });
    
    throw error;
  }
}
```

### Performance Monitoring

Monitor API performance metrics:

- Response times
- Error rates
- Credit usage
- Cache hit/miss ratios

## User Experience

### Loading States

Always implement loading states in your UI:

```jsx
function TwitterProfile({ username }) {
  const [profile, setProfile] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    async function loadProfile() {
      setLoading(true);
      try {
        const data = await api.getTwitterUserDetails(username);
        setProfile(data);
        setError(null);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }
    
    loadProfile();
  }, [username]);
  
  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage message={error} />;
  
  return (
    <div className="profile">
      <h2>{profile.details.name}</h2>
      <p>{profile.details.bio}</p>
      {/* Other profile details */}
    </div>
  );
}
```

### Graceful Degradation

Implement fallback content for when API requests fail:

- Show cached data when available
- Display friendly error messages
- Provide retry options for failed requests

## Next Steps

- [Getting Started Guide](getting-started.md)
- [Client Libraries](client-libraries.md)
- [API Reference](../api/overview.md)