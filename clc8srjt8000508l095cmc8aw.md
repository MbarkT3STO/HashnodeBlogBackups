# HTTP Status Codes

HTTP status codes are three-digit codes that are used to indicate the status of a request made to a server. These codes are sent by the server in response to a client's request and are used to indicate whether the request was successful, whether the server was able to fulfill the request, or if there was an error.

## **1xx Informational**

The 1xx (Informational) class of status code indicates that the request has been received and the process is continuing. This class of status code is intended for use with provisional responses, and the server must not send a final response until the request has been completed.

Examples of 1xx status codes include:

* 100 Continue: This status code indicates that the server has received the request headers and that the client should send the request body.
    
* 101 Switching Protocols: This status code indicates that the server is willing to switch to a different protocol, such as upgrading from HTTP/1.1 to HTTP/2.
    

## **2xx Success**

The 2xx (Success) class of status code indicates that the request was successful and that the requested information has been sent in the response.

Examples of 2xx status codes include:

* 200 OK: This is the most common status code and indicates that the request was successful.
    
* 201 Created: This status code indicates that the request was successful and that a new resource has been created as a result.
    
* 204 No Content: This status code indicates that the request was successful, but that there is no additional information to send back.
    

## **3xx Redirection**

The 3xx (Redirection) class of status code indicates that further action is required in order to complete the request. These codes are typically used when the server needs to redirect the client to a different location in order to complete the request.

Examples of 3xx status codes include:

* 301 Moved Permanently: This status code indicates that the resource has been permanently moved to a new location and that all future requests should be directed to the new location.
    
* 302 Found: This status code indicates that the resource has been temporarily moved to a new location and that future requests should be directed to the new location.
    
* 304 Not Modified: This status code indicates that the client's cached version of the resource is still up-to-date and that the server does not need to send a copy of the resource.
    

## **4xx Client Error**

The 4xx (Client Error) class of status code indicates that the request made by the client was invalid or cannot be fulfilled.

Examples of 4xx status codes include:

* 400 Bad Request: This status code indicates that the request made by the client was invalid or malformed.
    
* 401 Unauthorized: This status code indicates that the client must authenticate itself to get the requested response.
    
* 404 Not Found: This status code indicates that the requested resource could not be found on the server.
    

## **5xx Server Error**

The 5xx (Server Error) class of status code indicates that the server encountered an error while trying to fulfill the request.

Examples of 5xx status codes include:

* 500 Internal Server Error: This status code indicates that an unexpected condition was encountered by the server and that no more specific message is available.
    
* 503 Service Unavailable: This status code indicates that the server is currently unable to handle the request due to maintenance or capacity issues.
    

### **HTTP Status Code Range**

HTTP status codes range from 100 to 599. The first digit of the status code indicates the class of the status code, as described above. The second and third digits give more specific information about the status of the request. For example, a status code of 404 indicates that the requested resource could not be found, while a status code of 500 indicates an internal server error.

### **HTTP Status Code Best Practices**

It is important for developers to use the appropriate status codes when building their applications. Some best practices for using HTTP status codes include:

* Use the correct status code for the request: It is important to use the correct status code for the specific request being made. For example, a 200 status code should only be used to indicate that a request was successful, and a 404 status code should only be used to indicate that a requested resource could not be found.
    
* Provide a helpful message or response with the status code: Along with the status code, it can be helpful to provide a message or response that explains the status code and any additional information that may be useful. This can help users and developers understand what is happening and troubleshoot any issues that may arise.
    
* Use redirects appropriately: Redirects (status codes 3xx) can be useful for directing users to the correct location, but they should be used sparingly and only when necessary. Overuse of redirects can slow down the user experience and make it difficult for users to find the information they are looking for.