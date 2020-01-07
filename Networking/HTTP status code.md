# HTTP response status code
The first digit of the status code specifies one of five standard classes of responses. All HTTP response status codes are separated into five classes (or categories). The first digit of the status code defines the class of response. The last two digits do not have any class or categorization role. There are five values for the first digit:

 - 1xx (Informational): The request was received, continuing process
 - 2xx (Successful): The request was successfully received, understood and accepted
 - 3xx (Redirection): Further action needs to be taken in order to complete the request
 - 4xx (Client Error): The request contains bad syntax or cannot be fulfilled
 - 5xx (Server Error): The server failed to fulfill an apparently valid request

# Common codes and its meaning
Response code | Meaning
------------- | -------
200 OK | Request been done. Return result.
201 Created | Request been done. New resource been created.
204 Accepted | Request accepted. No result.
- | -
400 Bad request | The server cannot or will not process the request due to an apparent client error (e.g., malformed request syntax or invalid request message framing
401 Unauthorised | The request was valid, but the server is refusing action, specifically for use when authentication is required and has failed or has not yet been provided.
403 Forbidden | The request was valid, but the server is refusing action.
404 Not found | Requested resources not found.
409 Conflict | Conflict of editing the same resource.
429 Too many request | The user has sent too many requests in a given amount of time.