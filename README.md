# custom-directory
Documentation on how to create a custom directory for Opensense to ingest.

# Notes:
Each request should return a JSON response with the following structure:
```
{
  data: [ ... ], // Array of objects
  links: {
    next: '/path/to/next/page'
}
```
Opensense will pass an Authorization header with a token to authenticate.
We will also pass a `pageSize` parameter.

# Example of paginated results
GET /users?pageSize=100

RESPONSE 1:
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
    next:  '/users?skip=100&limit=100'
  }
}
```


GET /users?skip=100&limit=100
RESPONSE 2:

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
    next:  '/users?skip=200&limit=100'
  }
}
```


GET /users?skip=200&limit=100
RESPONSE 3:
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
