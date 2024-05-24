# Opensense Custom Directory
Documentation on how to create a custom directory for Opensense to ingest.  

# Notes:
Each request should return a JSON response with the following structure:
```
{
  data: [ ... ], // Array of objects with custom fields
  links: {
    next: '/path/to/next/page'
}
```
Opensense will pass an `Authorization` header with a token to authenticate. eg, `Authorization: Bearer <token>`
We will also pass a `pageSize` parameter on the URL.

### Email attribute required
Each data object must have an `email` attribute.

### Field names
Each data object can have data fields.  The data field names must be lowercase characters and underscores only.  eg: `title`, `linkedin_url`.

### Timeouts
Each request should take no more than 30 seconds.  The maximum page size should be 1000 users.

### HTTPS required 
Your custom directory endpoint must have a valid TLS/SSL certificate

# Example of paginated results
### REQUEST FIRST PAGE
GET https://customdir.customer.com/users?pageSize=100

#### RESPONSE FIRST PAGE:
```
{
  data: [ 
    {
      email: 'user1@customer.com',
      name: 'Joe',
      lastUpdated:
    }, 
    {
      email: 'user2@customer.com',
      name: 'Joe',
    }, 
    ... 100 
  ],
  links: {
    next:  'https://customdir.customer.com/users?skip=100&limit=100'
  }
}
```

### REQUEST SECOND PAGE
GET https://customdir.customer.com/users?skip=100&limit=100

#### RESPONSE SECOND PAGE:

```
{
  data: [ 
    {
      email: 'user101@customer.com',
      name: 'Joe',
    }, 
    {
      email: 'user102@customer.com',
      name: 'Joe',
    }, 
    ... 100 
  ],
  links: {
    next:  'https://customdir.customer.com/users?skip=200&limit=100'
  }
}
```

### REQUEST LAST PAGE
GET https://customdir.customer.com/users?skip=200&limit=100
#### RESPONSE LAST PAGE:
```
{
  data: [ 
      {
        email: 'user201@customer.com',
        name: 'Joe',
      }, 
      {
        email: 'user202@customer.com',
        name: 'Joe',
      }, 
      ... end of list
  ],
  links: {
    next:  null, ''
  }
}
```
