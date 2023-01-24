# Working with HTTP Files in Visual Studio Code

Working with HTTP files in Visual Studio Code (VS Code) can be a powerful tool for testing and debugging APIs. These files, often with the extension .http, contain requests in the HTTP format that can be sent to an API to test its functionality. In this article, we will explore how to work with HTTP files in VS Code, including how to create and edit them, as well as how to send requests to an API using these files.

## **Creating and Editing HTTP Files**

To create a new HTTP file in VS Code, simply right-click in the explorer pane and select "New File." Then, give the file a name with the .http extension, for example, "testAPI.http". Once the file is created, you can begin editing it by adding requests in the HTTP format.

The format for a request in an HTTP file is as follows:

```http
[HTTP Method] [API Endpoint] [HTTP Version]
[Headers]

[Request Body (if applicable)]
```

For example, a GET request to an endpoint "[**https://example.com/api**](https://example.com/api)" would look like this:

```http
GET https://example.com/api HTTP/1.1
Accept: application/json
```

It's also possible to add variables in the request by using the `{{variable}}` syntax. For example, to add a variable for the endpoint in the previous example:

```http
GET {{endpoint}} HTTP/1.1
Accept: application/json
```

## **Sending Requests**

Once you have created and edited your HTTP file, you can send the requests to an API using the "[REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)" extension for VS Code. To install this extension, open the Extensions pane in VS Code and search for "REST Client". Once it is installed, you can open your HTTP file and send requests by clicking on the "Send Request" button at the top of the file.

You can also use variables in the request by adding a `vars.json` file in the same directory as your HTTP file. This file should contain a JSON object with the variables and their corresponding values. For example, to use the "endpoint" variable from the previous example:

```http
{
  "endpoint": "https://example.com/api"
}
```

## Ignoring HTTP Version in Requests

When working with HTTP files in VS Code, it's possible to ignore the HTTP version in the requests. This can be useful when you don't need to specify a specific version of the HTTP protocol, or when the API you are testing automatically handles the version.

To ignore the HTTP version, simply remove the "HTTP/1.1" or "HTTP/2" from the request. For example, instead of:

```http
GET https://example.com/api HTTP/1.1
Accept: application/json
```

You can write:

```http
GET https://example.com/api
Accept: application/json
```

It's also possible to ignore the HTTP version for multiple requests by removing it from each request in the file. This can be done by using a search and replace function in VS Code to quickly remove the HTTP version from all requests in the file.

Keep in mind that some APIs might require a specific version of the HTTP protocol to work properly, so it's essential to check the documentation of the API before ignoring the HTTP version.

## **Advanced Features**

HTTP files in VS Code can also be used for more advanced testing scenarios, such as testing API authentication and authorization. To test API authentication, you can add the appropriate headers to your requests, such as "Authorization: Bearer \[token\]" for a JSON Web Token (JWT) authentication system.

In addition, you can also use the "REST Client" extension to save and reuse authentication tokens. This can be done by adding a `config` file in the same directory as your HTTP file. This file should contain a JSON object with the configuration settings. For example, to save and reuse an authentication token:

```http
{
  "saveAuthentication": true
}
```

Another advanced feature is the ability to test API endpoints that require multiple requests. For example, an API that requires a user to login before accessing other endpoints. To do this, you can add multiple requests in the same HTTP file, and use the `#` symbol to comment out requests that have not yet been executed. For example:

```http
# Login Request
POST https://example.com/api/login HTTP/1.1
Content-Type: application/json

{
  "username": "testuser",
  "password": "testpassword"
}

# Get User Profile Request
# GET https://example.com/api/user/profile HTTP/1.1
# Authorization: Bearer {{token}}
```

## **Working with Multiple Requests**

One of the great features of HTTP files is the ability to send multiple requests in the same file. This is especially useful when testing API endpoints that require multiple steps, such as logging in before accessing other endpoints. However, when working with multiple requests, it can be useful to separate them in the file for better organization and readability.

A common way to separate multiple requests in an HTTP file is by using the triple `###` symbol. This creates a visual separation between requests without commenting them out. For example:

```http
### Login Request
POST https://example.com/api/login HTTP/1.1
Content-Type: application/json

{
  "username": "testuser",
  "password": "testpassword"
}

### Get User Profile Request
GET https://example.com/api/user/profile HTTP/1.1
Authorization: Bearer {{token}}

### Update User Profile Request
PUT https://example.com/api/user/profile HTTP/1.1
Content-Type: application/json
Authorization: Bearer {{token}}

{
  "first_name": "John",
  "last_name": "Doe"
}
```

Using this method, you can easily separate and organize your requests in the same file, making it easier to read and understand the different steps of your tests.