# Parse Query Field Restrictions

## Requirements
This project depends on Parse Query Middleware, which is not supported by the official Parse Server project. Please use Parse Server from this fork: https://github.com/stefanraffeiner/parse-server

## Usage
This middleware can be used to hide specific fields of specific objects from specific users.

### Example
Class BlogPost contains following object.
```javascript
{
    "user": "A",
    "title": "Hello World",
    "comments": [{
        "author": "C",
        "message": "Hello"
    }],
    _rperm: ["userA", "userB"] // = read permissions via ACL
```

According to ACL, both user A, and user B can read this object. But we'd now like to hide the author of comments from user B. To do so we add following value to user B (class _User):

```javascript
userB.restrictedFields = [{
    "className": "BlogPost",
    "condition": {
        "field": "user",
        "value": "A"
    },
    "fields": ["comments.author"]
}]
```

This method only makes sens, if ACL or CLP block user B from updating its own _User object.