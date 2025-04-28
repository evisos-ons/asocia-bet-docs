# AsociaBet API Documentation

Welcome to the AsociaBet API - your gateway to accessing Twitter and YouTube data for your applications. Our API provides a simple, reliable, and secure way to integrate social media data into your products.

## Quick Integration Guide

1. [API Overview](api/overview.md) - Learn the basics of our API
2. [Get API Keys](api/authentication.md) - Obtain and use your API keys
3. [Explore Endpoints](api/endpoints/twitter.md) - Discover available data endpoints

## API Features

- **Social Media Data**: Access Twitter user details, followers, and tweets. Retrieve YouTube channel information and shorts.
- **Simple Authentication**: Easy-to-implement API key system with support for both server-side and client-side applications.
- **JSON Responses**: Clean, consistent JSON responses for seamless integration.
- **Reliable Rate Limiting**: Credit-based system to ensure fair usage and availability.
- **Comprehensive Documentation**: Clear examples and guides for quick implementation.

## Integration Examples

Integrate our API into your application with just a few lines of code:

```javascript
// Example: Fetch Twitter user details
const fetchUserDetails = async (username) => {
  const response = await fetch(`https://dev-api.asocia.bet/api/twitter/${username}/details`, {
    headers: {
      'X-API-Key': 'your-api-key'
    }
  });
  return response.json();
};
```

## Support

Need help with integration?

- Review our [API Reference](api/overview.md)
- Check out [Integration Best Practices](integration/best-practices.md)
- Contact our integration support at api-support@asociabet.com

## Get Started

Ready to add social media data to your application?

1. [Create an Account](https://dev-api.asocia.bet/signup)
2. [Get Your API Keys](api/authentication.md)
3. [Start Building](integration/getting-started.md)