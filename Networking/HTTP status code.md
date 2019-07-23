# 1xx Informational Response
Used only in experimental conditions.

# 2xx Success
## 200 OK
Request been done. Return result.
## 201 Created
Request been done. New resource been created.
## 204 Accepted
Request accepted. No result.

# 3xx Redirection
Many used in URL redirection.

# 4xx Client Errors
## 400 Bad request
Like invalid parameters.
## 401 Unauthorised
May need token or request for login.
## 403 Forbidden
The request was valid, but the server is refusing action.
## 404 Not found
Requested resources not found.
## 409 Conflict
Conflict of editing the same resource.
## 429 Too many request
The user has sent too many requests in a given amount of time.

# 5xx Server errors
## 500	Internal server error
## 502	Bad gateway
## 504	Gateway timeout