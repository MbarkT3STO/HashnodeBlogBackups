# Storing JWT Refresh Tokens in a Database using Identity and EF Core in ASP.NET Core

One important aspect of JWT usage is the handling of refresh tokens, which are used to obtain new access tokens after the original one expires. In this article, we will discuss how to store JWT refresh tokens in a database using Identity and EF Core in an [ASP.NET](http://ASP.NET) Core application.

## **Adding a Refresh Token Entity**

Next, we will create a new entity for our refresh tokens. In the `Models` folder, create a new class called `RefreshToken.cs` and add the following code:

```csharp
public class RefreshToken
{
    public string Token { get; set; }
    public string JwtId { get; set; }
    public DateTime CreationDate { get; set; }
    public DateTime ExpiryDate { get; set; }
    public bool Used { get; set; }
    public bool Invalidated { get; set; }
    public string UserId { get; set; }
    public AppUser User { get; set; }
}
```

This class represents a single refresh token and contains properties such as the token string, the JWT ID, and the expiration date. We also have a `UserId` property that will be used to link the refresh token to a specific user.

## **Creating the Database Context**

Now that we have our entity, we need to create a database context that will handle the interaction with the database. In the `Data` folder, create a new class called `AppDbContext.cs` and add the following code:

```csharp
public class AppDbContext : IdentityDbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
    { }

    public DbSet<RefreshToken> RefreshTokens { get; set; }
}
```

This class inherits from `IdentityDbContext` and adds a `DbSet` for our `RefreshToken` entity.

## **Configuring the Database Connection**

In the `appsettings.json` file, you need to configure the connection string to your database, like this:

```json
"ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=yourdbname;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
```

## **Registering the Database Context**

In the `Startup.cs` file, you need to register the database context. In the `ConfigureServices` method, add the following code:

```csharp
services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
```

This will configure the application to use the `AppDbContext` class as the database context and use the connection string from the `appsettings.json` file to connect to the database.

## **Adding the Refresh Token to the User**

Now that we have our database context set up, we can start adding refresh tokens to our users. In the `AppUser` class, add a new `ICollection` property for the refresh tokens:

```csharp
public class AppUser : IdentityUser
{
    public ICollection<RefreshToken> RefreshTokens { get; set; }
}
```

This allows us to access all the refresh tokens of a user.

## **Generating and Storing the Refresh Token**

We can now generate and store the refresh token in the database. In the `AccountController`, create a new method called `GenerateRefreshToken` and add the following code:

```csharp
private async Task<RefreshToken> GenerateRefreshToken(string userId)
{
    var refreshToken = new RefreshToken
    {
        JwtId = Guid.NewGuid().ToString(),
        UserId = userId,
        CreationDate = DateTime.UtcNow,
        ExpiryDate = DateTime.UtcNow.AddMonths(6)
    };

    _context.RefreshTokens.Add(refreshToken);
    await _context.SaveChangesAsync();

    return refreshToken;
}
```

This method generates a new refresh token and assigns it to the user with the specified ID. It also sets the creation date and expiry date of the token.

## **Using the Refresh Token**

Now that the refresh token is stored in the database, we can use it to obtain a new access token. In the `AccountController`, create a new method called `RefreshAccessToken` and add the following code:

```csharp
[HttpPost("refresh")]
public async Task<IActionResult> RefreshAccessToken([FromBody] RefreshTokenRequest request)
{
    var user = await _userManager.FindByIdAsync(request.UserId);
    if (user == null)
    {
        return BadRequest(new { message = "Invalid user" });
    }

    var refreshToken = _context.RefreshTokens.SingleOrDefault(rt => rt.Token == request.RefreshToken && rt.UserId == request.UserId);
    if (refreshToken == null)
    {
        return BadRequest(new { message = "Invalid refresh token" });
    }

    if (refreshToken.Used)
    {
        return BadRequest(new { message = "Refresh token already used" });
    }

    if (refreshToken.Invalidated)
    {
        return BadRequest(new { message = "Refresh token invalidated" });
    }

    if (refreshToken.ExpiryDate < DateTime.UtcNow)
    {
        return BadRequest(new { message = "Refresh token expired" });
    }

    refreshToken.Used = true;
    _context.RefreshTokens.Update(refreshToken);
    await _context.SaveChangesAsync();

    var accessToken = GenerateAccessToken(user);
    return Ok(new
    {
        access_token = accessToken,
        refresh_token = request.RefreshToken
    });
}
```

This method takes in a `RefreshTokenRequest` object which contains the refresh token and the user ID. It then checks if the user and refresh token exist in the database and if they haven't been used or invalidated and the refresh token isn't expired. If all these conditions are met, the refresh token is marked as used and a new access token is generated for the user.

Here's an example of how you can use the `RefreshAccessToken` method in an authorized action:

```csharp
[Authorize]
[HttpGet("secure")]
public IActionResult Secure()
{
    var currentUser = HttpContext.User;
    // Your secure logic here
    return Ok("Access granted");
}
```

In this example, the `Secure` action is decorated with the `Authorize` attribute, which means that only authorized users can access it. When a user makes a request to this action, the application will check the user's access token to verify their identity. If the access token is valid and has not expired, the user will be granted access to the action and the secure logic will be executed.

If the access token is expired, the user can use the refresh token to obtain a new access token. The user can make a request to the `RefreshAccessToken` action with the refresh token, and if the token is valid, the server will generate a new access token and return it to the user. The user can then use this new access token to make a request to the `Secure` action, and the server will grant them access.

Note that, you should implement the `RefreshTokenRequest` and `GenerateAccessToken` yourself.

It's important to note that in a production application, you should also handle cases such as invalidating the refresh token after a certain number of uses or a certain period of time to ensure that the tokens can't be used indefinitely.