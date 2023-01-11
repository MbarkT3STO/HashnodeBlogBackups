# How to Generate a JWT in ASP.NET Core API with Identity and EF Core?

We talked before about [What is JWT](https://mbarkt3sto.hashnode.dev/what-is-jwt) and [**What is JWT Bearer in**](https://mbarkt3sto.hashnode.dev/what-is-jwt-bearer-in-aspnet-core) [**ASP.Net**](http://ASP.Net) [**Core?**](https://mbarkt3sto.hashnode.dev/what-is-jwt-bearer-in-aspnet-core), but it is not a bad idea to repeat a brief introduction to it.

JSON Web Tokens (JWTs) are a compact, URL-safe means of representing claims to be transferred between two parties. They are often used for authentication and authorization purposes in web applications. In this article, we will cover how to generate a JWT in an [ASP.Net](http://ASP.Net) Core application using Microsoft Identity and EF Core.

Before we begin, it's important to note that in order to follow along with this article, you will need to have an existing [ASP.Net](http://ASP.Net) Core application that is configured to use Identity Core and EF Core. Additionally, you will need to have some basic knowledge of C# and the [ASP.Net](http://ASP.Net) Core framework.

## **Step 1: Install Required Packages**

To get started, we need to install the necessary packages for generating JWT tokens. In the package manager console, run the following commands:

```csharp
Install-Package Microsoft.AspNetCore.Authentication.JwtBearer
Install-Package Microsoft.AspNetCore.Identity
```

This will add the required dependencies to generate and validate JWT in our application, in addition to Identity.

### **Defining the data model**

Next, we need to define the data model that we will use to store user information. This can be done by creating a new class that derives from `IdentityUser` and adding any additional properties that you need.

```csharp
using Microsoft.AspNetCore.Identity;

public class ApplicationUser : IdentityUser
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

You will also need to create a DbContext that will be used to interact with the database.

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

## **Step 2: Add JWT Bearer Authentication**

Once the required packages are installed, we need to add the JWT bearer authentication to our application. Open the `Startup.cs` file and add the following code to the `ConfigureServices` method:

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = Configuration["Jwt:Issuer"],
            ValidAudience = Configuration["Jwt:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(Configuration["Jwt:Key"]))
        };
    });
```

This code sets up JWT bearer authentication in our application, and it specifies that the issuer, audience, and signing key for the JWT should be read from the `appsettings.json` file using the `Configuration` object.

In your `appsettings.json` file, you would define the values for the "Jwt:Issuer", "Jwt:Audience", and "Jwt:Key" configuration keys. The exact structure of the `appsettings.json` file will depend on your application's configuration, but a basic example of what it might look like with the JWT configuration keys included would be as follows:

```json
{
  "Jwt": {
    "Issuer": "your_issuer",
    "Audience": "your_audience",
    "Key": "your_secret_key"
  },
  "ConnectionStrings": {
    "DefaultConnection": "Your database connection here"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

It should be noted that, for security reasons, the key for signing the JWT should be kept secret and should not be checked in source control or stored in plain text on the server. Instead, it should be passed as an environment variable or by some other secure means.

Also, the Jwt:Issuer & Jwt:Audience values usually are URIs or domain names that identify your application as the entity that issued the JWT, and the entity for which the JWT is intended, respectively. But that could be whatever you want or need.

### Other Required Services

```csharp
services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

services.AddIdentity<ApplicationUser, IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

The above code will register our `ApplicationDbContext` and `Identity` services with the DI container, and configure the `ApplicationDbContext` to use a SQL Server database using the connection string named `DefaultConnection` from the `appsettings.json` file.

### **Configuring authentication**

Now that we have registered the services, we need to configure our API to use them for authentication. This can be done by adding the following code to the `Configure` method in the `Startup.cs` file:

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

This will add the `Authentication` and `Authorization` middleware to the request pipeline, which will automatically handle the authentication and authorization of requests based on the configuration we provided earlier.

## **Step 3: Create the Token**

Next, we need to create the JWT token. To do this, we will create a new controller called `AuthController` and add the following code to the `Login` action:

```csharp
[HttpPost("token")]
public async Task<IActionResult> Login([FromBody]LoginModel model)
{
    var user = await _userManager.FindByNameAsync(model.Username);
    if (user != null && await _userManager.CheckPasswordAsync(user, model.Password))
    {
        var claims = new[]
        {
            new Claim(JwtRegisteredClaimNames.Sub, user.Id.ToString()),
            new Claim(JwtRegisteredClaimNames.UniqueName, user.UserName)
        };
        var signingKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("MySuperSecureKey"));
        var signingCredentials = new SigningCredentials(signingKey, SecurityAlgorithms.HmacSha256);
        var jwt = new JwtSecurityToken(
            signingCredentials: signingcredentials,
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(30)
        );

        var token = new JwtSecurityTokenHandler().WriteToken(jwt);
        return Ok(new { token });
    }
    else
    {
        return Unauthorized();
    }    
}
```

This code retrieves the user from the database using the `_userManager` object, which is an instance of `UserManager<ApplicationUser>`. If the user exists and the provided password is correct, the code creates a new JWT with the user's id and username as claims.

It also set a expiration time of 30 minutes after the token generation. It then writes the token using a `JwtSecurityTokenHandler` object and returns the token as a response to the client.

It is important to note that the code above is for demonstration purposes and in practice, you should use a more secure method of storing the key used to sign the JWT and use a secure key and do not hardcode it as in the example above.

The `LoginModel` class is a C# model class that is used to represent the data that is sent to the server in a login request. It could be something like this:

```csharp
public class LoginModel
{
    public string Username { get; set; }
    public string Password { get; set; }
}
```

It contains two properties, `Username` and `Password`, both of which are strings, to receive the login credentials from the client. The client would typically send this data as a JSON object in the request body. And then in the controller action method, this class is decorated with the `FromBody` attribute to indicate that the data should be read from the request body.

It could have other properties as well, like Email or any other property related to your authentication system. You could also have validation rules in the class properties using DataAnnotations to ensure that the input data is valid, but I left them out of this example for brevity.

## A Real-World Example

It's great that you would like to have a complete real-world example of JWT in [ASP.NET](http://ASP.NET) Core API with Identity Core and EF Core, so I will provide you with an example that you can use as a starting point for your own project. Please note that you may need to adjust the code for your specific requirements and best practice.

```csharp
using System;
using System.IdentityModel.Tokens.Jwt;
using System.Linq;
using System.Security.Claims;
using System.Text;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;
using Microsoft.IdentityModel.Tokens;

namespace JwtExample.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class AuthController : ControllerBase
    {
        private readonly SignInManager<ApplicationUser> _signInManager;
        private readonly UserManager<ApplicationUser> _userManager;
        private readonly IConfiguration _configuration;

        public AuthController(
            UserManager<ApplicationUser> userManager,
            SignInManager<ApplicationUser> signInManager,
            IConfiguration configuration
        )
        {
            _userManager = userManager;
            _signInManager = signInManager;
            _configuration = configuration;
        }

        [HttpPost("login")]
        public async Task<IActionResult> Login([FromBody] LoginModel model)
        {
            var result = await _signInManager.PasswordSignInAsync(model.Email, model.Password, false, false);

            if (result.Succeeded)
            {
                var appUser = _userManager.Users.SingleOrDefault(r => r.Email == model.Email);
                var token = GenerateJwtToken(appUser);
                return Ok(new
                {
                    token = token,
                    expiration = token.ValidTo,
                });
            }

            return Unauthorized();
        }

        [HttpPost("register")]
        public async Task<IActionResult> Register([FromBody] RegisterModel model)
        {
            var user = new ApplicationUser
            {
                UserName = model.Email,
                Email = model.Email
            };
            var result = await _userManager.CreateAsync(user, model.Password);

            if (result.Succeeded)
            {
                await _signInManager.SignInAsync(user, false);
                var token = GenerateJwtToken(user);
                return Ok(new
                {
                    token = token,
                    expiration = token.ValidTo,
                });
            }

            return BadRequest(result.Errors);
        }

        private JwtSecurityToken GenerateJwtToken(ApplicationUser user)
        {
            var claims = new List<Claim>
            {
                new Claim(JwtRegisteredClaimNames.Sub, user.Id)
            };

            var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["JwtKey"]));
            var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
            var expires = DateTime.Now.AddDays(Convert.ToDouble(_configuration["JwtExpireDays"]));

            var token = new JwtSecurityToken(
                _configuration["JwtIssuer"],
                _configuration["JwtAudience"],
                claims,
                expires: expires,
                signingCredentials: creds
            );

            return token;
        }
```

In this code snippet, we are using the `SymmetricSecurityKey`, `SigningCredentials`, `JwtIssuer`, `JwtAudience`, and `JwtExpireDays` that are configured in the appsettings.json file, to create and sign a JWT token. We also added the user's id as a subject claim to the token. This token is then returned from the method.

In addition to the `Login` and `Register` methods, you could also add a `ValidateToken` method to check if a token is valid and has not expired.

```csharp
[HttpGet("validate")]
public IActionResult Validate()
{
    var token = Request.Headers["Authorization"].ToString().Replace("Bearer ", "");

    try
    {
        var claimsPrincipal = new JwtSecurityTokenHandler().ValidateToken(token, GetTokenValidationParameters(), out var validatedToken);

        return Ok("Token is valid");
    }
    catch (SecurityTokenExpiredException)
    {
        return Unauthorized("Token has expired");
    }
    catch (Exception)
    {
        return Unauthorized("Token is invalid");
    }
}

private TokenValidationParameters GetTokenValidationParameters()
{
    var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["JwtKey"]));
    var issuer = _configuration["JwtIssuer"];
    var audience = _configuration["JwtAudience"];

    return new TokenValidationParameters
    {
        ValidateIssuerSigningKey = true,
        IssuerSigningKey = key,
        ValidateIssuer = true,
        ValidIssuer = issuer,
        ValidateAudience = true,
        ValidAudience = audience,
        ClockSkew = TimeSpan.Zero
    };
}
```

This is just an example, you should also include other security measures like rate limiting and include other validation/sanitation to protect your API from malicious requests.

## Conclusion

In this article, we've shown you how to generate JSON Web Tokens (JWT) in an [ASP.Net](http://ASP.Net) Core application using Identity Core and EF Core. By following the steps outlined in this article, you should be able to create a JWT that can be securely passed between the client and server in your application.

Keep in mind that this article has provided a brief overview and it's important to check the official documentation to have a more in-depth understanding of the topic.