# http status code

- **100 Continue**
This interim response indicates that everything so far is OK and that the client should continue the request, or ignore the response if the request is already finished.
- **200 OK**
The request has successded. The meaning of the success depends on the HTTP method:
- **301 Moved Permanently**
The URL of the requested resource has been changed permanently. The new URL is given in the response.
- **302 Found**
This response code means that the URI of requested resource has been changed temporarily. Further changes in the URI might be made in the future. Therefore, this same URI should be used by the client in future requests.
- **304 Not Modified**
This is used for caching purposes. It tells the client that the response has not been modified, so the client can continue to use the same cached version of the response.
- **307 Temporary Redirect**
The server sends this response to direct the client to get the requested resource at another URI with same method that was used in the prior request. This has the same semantics as the **302 Found** HTTP response code, with the exception that the user agent must not change the HTTP method used: If a **POST** was used in the first request, a **POST** must be used in the second request.
- **400 Bad Request**
The server could not understand the request due to invalid syntax
- **401 Unauthorized**
Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response.
- **403 Forbidden**
The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike 401, the client's identity is known to the server.
- **404 Not Found**
The server can not find requested resource. In the browser, this means the URL is not recognized. In an API, this can also mean that the endpoint is valid but the reource itself does not exist. Servers may also send this response instead of 403 to hide the existence of a resource form an unauthorized client. This response code is probably the most famous one due to its frequent occurrence on the web.
- **405 Method Not Allowed**
The request method is known by the server but has been disabled and cannot be used. For exampe, an API may forbid DELETE-ing a resource. The Two mandatory methods, **GET** and **HEAD**, must never be disabled and should not reutrn this error code.
- **406 Not Acceptable**
This response is sent when the web server, after performing server-driven content negotiation, doesn't find any content that conforms to the criteria given by the user agent.
- **500 Internal Server Error**
The server has encountered a situation it doesn't know how to handle.
- **502 Bad Gateway**
This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.
- **503 Service Unavailable**
The server is not ready to handle the request. Common causes are a server that is down for maintenance or that is overloaded. Note that together with this response, a user-friendly page explaining the problem should be sent. This responses should be used for temporary conditions and the **Retry-After**: HTTP header should, if possible, contain the estimated time before the recovery of the service. The webmaster must also take care about the caching-related headers that are sent along with this response, as these temporary condition responses should usually not be cached.
- **504 Gateway Timeout**
The server was acting as a gateway or proxy and did not receive a timely response from the upstream server.

## 引用

- [Mozilla: HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
