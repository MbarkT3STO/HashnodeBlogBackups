# What is JWT Bearer in ASP.Net Core?

One feature that is often used in [ASP.NET](http://ASP.NET) Core is JSON Web Tokens (JWTs) for authentication and authorization. In this article, we will explain what JWT Bearer is and how to use it in an [ASP.NET](http://ASP.NET) Core application.

## **What is a JSON Web Token (JWT)?**

A JSON Web Token (JWT) is a JSON-based open standard that is used to securely transmit information between parties. The information contained in a JWT is encoded as a JSON object and signed using a digital signature.

JWTs are often used as a means of authentication and authorization in web applications. They are self-contained, meaning that they contain all the necessary information about the user and their permissions in a single token. This makes them easy to use and maintain, as there is no need to store user information in a database or other external system.

## **What is JWT Bearer in** [**ASP.NET**](http://ASP.NET) **Core?**

JWT Bearer is a authentication scheme that allows a client to send a JSON Web Token (JWT) in the Authorization header of an HTTP request. This is often used in combination with OAuth 2.0 to authenticate users in a web application.

The JWT Bearer scheme is implemented in [ASP.NET](http://ASP.NET) Core using the `Microsoft.AspNetCore.Authentication.JwtBearer` package. This package provides a set of middleware that can be added to an [ASP.NET](http://ASP.NET) Core application to enable JWT Bearer authentication.

## **How to use JWT Bearer in** [**ASP.NET**](http://ASP.NET) **Core**

To use JWT Bearer in an [ASP.NET](http://ASP.NET) Core application, you will need to follow these steps:

1. Install the `Microsoft.AspNetCore.Authentication.JwtBearer` package using the NuGet Package Manager or by running the following command in the Package Manager Console:
    

```csharp
Install-Package Microsoft.AspNetCore.Authentication.JwtBearer
```

1. In the `Startup.cs` file, add the following lines of code to the `ConfigureServices` method to add the JWT Bearer authentication middleware to your application:
    

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Authority = "https://your-authority.com";
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidIssuer = "your-issuer",
                ValidAudience = "your-audience",
                IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("your-secret-key"))
            };
        });
```

1. In the `Configure` method, add the following line of code to enable the JWT Bearer authentication middleware:
    

```csharp
app.UseAuthentication();
```

1. To protect a specific route or controller in your application, add the `[Authorize]` attribute to the route or controller. For example:
    

```csharp
[Authorize]
public class MyController : Controller
{
    // Your controller actions go here
}
```

1. To generate a JWT for a user, you will need to create a function that takes the user's credentials and generates a JWT. This will typically involve creating a JSON payload with the necessary information about the user and signing the payload using a secret key. Here is an example of a function that generates a JWT in C#:
    

```csharp
private string GenerateJWT(string username, string password)
{
    var claims = new[]
    {
    new Claim(JwtRegisteredClaimNames.Sub, username),
    new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
    };
    
    var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("your-secret key"));
    var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
    
    var token = new JwtSecurityToken(
        issuer: "your-issuer",
        audience: "your-audience",
        claims: claims,
        expires: DateTime.Now.AddMinutes(30),
        signingCredentials: creds);
    
    return new JwtSecurityTokenHandler().WriteToken(token);
}
```

You can then send the generated JWT to the client and store it in a secure way (such as in an HTTP-only cookie). The client can then send the JWT in the `Authorization` header of subsequent requests to authenticate the user.

```csharp
Authorization: Bearer <your-jwt>
```

## Conclusion

JWT Bearer is a powerful authentication scheme that allows you to authenticate and authorize users in an [ASP.NET](http://ASP.NET) Core application using JSON Web Tokens. By following the steps outlined above, you can easily add JWT Bearer authentication to your application and secure your routes and controllers.