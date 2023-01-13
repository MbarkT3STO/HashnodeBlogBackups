# A Comprehensive Example: Implementing JWT Authentication in ASP.NET Core with Identity and EF Core

We talked before about :

[**What is JWT?**](https://mbarkt3sto.hashnode.dev/what-is-jwt)  
[**What is JWT Bearer in**](https://mbarkt3sto.hashnode.dev/what-is-jwt-bearer-in-aspnet-core) [**ASP.Net**](http://ASP.Net) [**Core?**](https://mbarkt3sto.hashnode.dev/what-is-jwt-bearer-in-aspnet-core)  
[**How to Generate a JWT in**](https://mbarkt3sto.hashnode.dev/how-to-generate-a-jwt-in-aspnet-core-api-with-identity-and-ef-core) [**ASP.NET**](http://ASP.NET) [**Core API with Identity and EF Core?**](https://mbarkt3sto.hashnode.dev/how-to-generate-a-jwt-in-aspnet-core-api-with-identity-and-ef-core)

In this article, we will take a complete example of using JWT in [ASP.NET](http://ASP.NET) Core with Identity and EF Core.

**Note:** It is necessary to read the JWT articles I mentioned above, as well as having a basic understanding of [`ASP.NET`](http://ASP.NET) `Core` with `Identity` and `Entity Framework Core (EF Core)`, and also a basic knowledge of `Angular`. All of the aforementioned basic understanding is required to fully comprehend this article.

## **Setting up the Project**

To get started, create a new [ASP.NET](http://ASP.NET) Core web application and select the "Web API" template. Then, install the following packages:

* `Microsoft.AspNetCore.Identity`
    
* `Microsoft.AspNetCore.Authentication.JwtBearer`
    
* `Microsoft.EntityFrameworkCore`
    
* `Microsoft.EntityFrameworkCore.SqlServer`
    
* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`
    
* `Microsoft.EntityFrameworkCore.Design`
    

We will be using SQL Server as the database for this example, but EF Core supports many other database providers as well.

## **Creating the Student Entity**

Next, create a new folder called "Entities" and add the following class for the Student entity:

```csharp
public class Student
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

Then, create a new folder called "Data" and add a new class called "ApplicationDbContext" that inherits from `IdentityDbContext<IdentityUser>` and also has a DbSet for the Student entity.

```csharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser>
{
    public DbSet<Student> Students { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=JWTExample;Trusted_Connection=True;");
    }
}
```

## **Configuring JWT Authentication**

In the `Startup` class, add the following code to the `ConfigureServices` method to configure JWT authentication:

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
                ValidIssuer = "yourdomain.com",
                ValidAudience = "yourdomain.com",
                IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("yoursecretkey"))
            };
        });
```

Don't forget to replace "[yourdomain.com](http://yourdomain.com)" and "yoursecretkey" with the actual values for your application.

## **Creating the StudentController**

Create a new folder called "Controllers" and add a new class called "StudentController" that inherits from `ControllerBase` and has the following actions:

```csharp
[HttpPost("register")]
public async Task<IActionResult> Register([FromBody] Student student)
{
    // create user
    var user = new IdentityUser { UserName = student.Email, Email = student.Email };
    var result = await _userManager.CreateAsync(user, "P@ssw0rd");
    if (!result.Succeeded)
    {
        return BadRequest();
    }

    // add student to db
    _dbContext.Students.Add(student);
    await _dbContext.SaveChangesAsync();

    // generate jwt token
    var claims = new[]
    {
        new Claim(JwtRegisteredClaimNames.Sub, student.Email),
        new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
    };

    var token = new JwtSecurityToken(
        issuer: "yourdomain.com",
        audience: "yourdomain.com",
        claims: claims,
        expires: DateTime.UtcNow.AddMinutes(30),
        signingCredentials: new SigningCredentials(new SymmetricSecurityKey(Encoding.UTF8.GetBytes("yoursecretkey")), SecurityAlgorithms.HmacSha256)
    );

    // return jwt token
    return Ok(new
    {
        token = new JwtSecurityTokenHandler().WriteToken(token),
        expiration = token.ValidTo
    });
}
```

This action will handle the registration of new students. It first creates a new user with the Identity framework, then adds the student to the database, and finally generates a JWT token and returns it to the client.

## **Using the** `Authorize` Attribute

In order to restrict access to a specific action, you can use the `Authorize` attribute. This attribute can be applied to a controller class or to an individual action method. When applied to a controller class, all actions within that controller will be protected by the authorization rules. When applied to an action method, only that specific action will be protected.

For example, if you have an action in your `StudentController` that returns a list of students, you can use the `Authorize` attribute to restrict access to that action to only authenticated users:

```csharp
[Authorize]
[HttpGet("students")]
public IActionResult GetStudents()
{
    var students = _dbContext.Students.ToList();
    return Ok(students);
}
```

This action will now only be accessible to users who have been authenticated and have a valid JWT token. If an unauthenticated user attempts to access this action, they will receive a `401 Unauthorized` response.

You can also use the `Authorize` attribute with roles or policies.For example, if you have a role called "admin", you can use the `Authorize` attribute with the role name to allow only users with "admin" role to access this action.

```csharp
[Authorize(Roles = "admin")]
[HttpGet("students")]
public IActionResult GetStudents()
{
    var students = _dbContext.Students.ToList();
    return Ok(students);
}
```

This way, only users with the "admin" role will be able to access this action. Other users will receive a `401 Unauthorized` response.

By using the `Authorize` attribute, you can easily control access to your actions and ensure that only authorized users can access sensitive data or perform specific actions.

## **Checking and Validating Access to the** `GetStudents` Action

There are several ways to check and validate access to the `GetStudents` action, which is protected by the `Authorize` attribute.

### **Using the** `User` Property

The `User` property of the `Controller` or `ControllerBase` class provides access to the current user's claims and identity. You can use this property to check if the user is authenticated and to check for specific roles or claims.

For example, you can use the `IsInRole` method of the `User` property to check if the user has a specific role before returning the students list:

```csharp
[Authorize]
[HttpGet("students")]
public IActionResult GetStudents()
{
    if (User.IsInRole("admin"))
    {
        var students = _dbContext.Students.ToList();
        return Ok(students);
    }
    else
    {
        return Unauthorized();
    }
}
```

This way, only users with the "admin" role will be able to access this action and see the students list, other users will receive a `401 Unauthorized` response.

You can also use the `Claims` property to check for specific claims:

```csharp
[Authorize]
[HttpGet("students")]
public IActionResult GetStudents()
{
    if (User.Claims.Any(c => c.Type == ClaimTypes.Email && c.Value == "admin@example.com"))
    {
        var students = _dbContext.Students.ToList();
        return Ok(students);
    }
    else
    {
        return Unauthorized();
    }
}
```

This way, only users with claim type "email" and value "[**admin@example.com**](mailto:admin@example.com)" will be able to access this action and see the students list, other users will receive a `401 Unauthorized` response.

### **Using the** `IAuthorizationService`

You can also use the `IAuthorizationService` to check if a user has access to a specific action or resource. This service can be injected into a controller or service and provides methods for checking access and handling authorization failures.

For example, you can use the `AuthorizeAsync` method to check if a user has access to the `GetStudents` action:

```csharp
[Authorize]
[HttpGet("students")]
public async Task<IActionResult> GetStudents()
{
    var authorizationResult = await _authorizationService.AuthorizeAsync(User, null, "viewStudentsPolicy");

    if (authorizationResult.Succeeded)
    {
        var students = _dbContext.Students.ToList();
        return Ok(students);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

This way, the `AuthorizeAsync` method will check if the user has the "viewStudentsPolicy" policy, if the user has it, the method will return the students list, otherwise it will return a `401 Unauthorized` response.

## **Checking and Validating Access to the** `GetStudents` Action with JWT

When using JSON Web Tokens (JWT) for authentication, you can use the `Authorize` attribute to restrict access to the `GetStudents` action to only authenticated users who have a valid JWT token. Additionally, you can also use the claims stored in the JWT token to check for specific roles or permissions.

### **Using the** `Authorize` Attribute

The `Authorize` attribute can be applied to a controller class or to an individual action method. When applied to a controller class, all actions within that controller will be protected by the authorization rules. When applied to an action method, only that specific action will be protected.

For example, you can use the `Authorize` attribute to restrict access to the `GetStudents` action to only authenticated users:

```csharp
[Authorize]
[HttpGet("students")]
public IActionResult GetStudents()
{
    var students = _dbContext.Students.ToList();
    return Ok(students);
}
```

This action will now only be accessible to users who have been authenticated and have a valid JWT token. If an unauthenticated user attempts to access this action, they will receive a `401 Unauthorized` response.

## **Using Claims from JWT Token**

You can also use the claims stored in the JWT token to check for specific roles or permissions. For example, you can check for the `role` claim to check if the user has the `admin` role:

```csharp
[Authorize]
[HttpGet("students")]
public IActionResult GetStudents()
{
    var role = User.Claims.FirstOrDefault(c => c.Type == "role")?.Value;
    if (role == "admin")
    {
        var students = _dbContext.Students.ToList();
        return Ok(students);
    }
    else
    {
        return Unauthorized();
    }
}
```

This way, only users with the `admin` role will be able to access this action and see the students list, other users will receive a `401 Unauthorized` response.

You can also check for any other claim that you've added to the JWT token, for example you can check for a custom claim called `permissions` that hold an array of permissions.

```csharp
[Authorize]
[HttpGet("students")]
public IActionResult GetStudents()
{
    var permissions = User.Claims.FirstOrDefault(c => c.Type == "permissions")?.Value;
    if (permissions.Contains("view_students"))
    {
        var students = _dbContext.Students.ToList();
        return Ok(students);
    }
    else
    {
        return Unauthorized();
    }
}
```

This way, only users with the `view_students` permission will be able to access this action and see the students list, other users will receive a `401 Unauthorized` response.

## **Consuming the** `Register` Method from an Angular Component

In order to consume the `Register` method from an Angular component, you can use the `HttpClient` module to make a POST request to the endpoint that the method is exposed on, along with the student's data that you want to register.

First, you need to import the `HttpClient` module in your component:

```typescript
import { HttpClient } from '@angular/common/http';
```

Then you need to inject it in your component's constructor:

```typescript
constructor(private http: HttpClient) {}
```

Then you can use the [`http.post`](http://http.post) method to make a POST request to the endpoint that the `Register` method is exposed on, along with the student's data that you want to register.

```typescript
register(student: Student) {
  this.http.post('/api/student/register', student).subscribe(
    data => {
      console.log(data);
      // you can extract the token and expiration time from the data object
      // and store it in the local storage
    },
    error => {
      console.log(error);
    }
  );
}
```

It's important to note that the above code snippet assumes that the `Register` method is exposed on the `/api/student/register` endpoint.

It's also important to note that the above code snippet assumes that the `student` object passed to the `register` method has the same properties that the `Student` model in the server side, otherwise the server will not be able to deserialize the student object from the request.

In order to extract the token and expiration time from the response of the `register` method and store them in the local storage, you can access the `data` object in the `subscribe` method and extract the values of the `token` and `expiration` properties.

You can then use the `localStorage.setItem` method to store the token and expiration time in the local storage.

```typescript
register(student: Student) {
  this.http.post('/api/student/register', student).subscribe(
    data => {
      console.log(data);
      const token = data.token;
      const expiration = data.expiration;
      localStorage.setItem('token', token);
      localStorage.setItem('expiration', expiration);
    },
    error => {
      console.log(error);
    }
  );
}
```

You can also use the `JSON.stringify()` method to store the expiration date as a string, then you can parse it later on.

```typescript
register(student: Student) {
  this.http.post('/api/student/register', student).subscribe(
    data => {
      console.log(data);
      const token = data.token;
      const expiration = data.expiration;
      localStorage.setItem('token', token);
      localStorage.setItem('expiration', JSON.stringify(expiration));
    },
    error => {
      console.log(error);
    }
  );
}
```

By storing the token and expiration time in the local storage, you can use them later to authenticate requests and keep the user logged in even after the browser is closed.

It's important to note that the `localStorage` is vulnerable to XSS attacks, you should consider using a secure way to store the token like using the `HttpOnly` and `secure` flags on the cookie or using an alternative storage like `sessionStorage` or an in-memory storage.

## **Consuming the** `GetStudents` Method from an Angular Component

In order to consume the `GetStudents` method from an Angular component, you can use the `HttpClient` module to make a GET request to the endpoint that the method is exposed on.

First, you need to import the `HttpClient` module in your component:

```typescript
import { HttpClient } from '@angular/common/http';
```

Then you need to inject it in your component's constructor:

```typescript
constructor(private http: HttpClient) {}
```

Then you can use the `http.get` method to make a GET request to the endpoint that the `GetStudents` method is exposed on. You can also subscribe to the observable returned by the `get` method to handle the response and any errors that may occur.

```typescript
ngOnInit() {
  this.http.get<Student[]>('/api/student/students').subscribe(
    data => {
      this.students = data;
    },
    error => {
      console.log(error);
    }
  );
}
```

It's important to note that the above code snippet assumes that the `GetStudents` method is exposed on the `/api/student/students` endpoint.

Also, you need to make sure that you are sending the JWT token with the request, this could be done by adding the token in the header of the request.

```typescript
ngOnInit() {
  this.http.get<Student[]>('/api/student/students', {
    headers: new HttpHeaders({
      'Authorization': `Bearer ${localStorage.getItem('token')}`
    })
  }).subscribe(
    data => {
      this.students = data;
    },
    error => {
      console.log(error);
    }
  );
}
```

This way, the `Authorization` header is added to the request with the token stored in the local storage, this ensures that the request is authorized and the server will return the students list.

You also can handle errors or unauthorized requests by using the `catchError` operator and the `throwError` function of the `rxjs` library

```typescript
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

ngOnInit() {
  this.http.get<Student[]>('/api/student/students', {
    headers: new HttpHeaders({
      'Authorization': `Bearer ${localStorage.getItem('token')}`
    })
  }).pipe(
    catchError(err => {
      if (err.status === 401) {
        // handle unauthorized request
      } else {
        return throwError(err);
      }
    })
  ).subscribe(
    data => {
      this.students = data;
    },
    error => {
      console.log(error);
    }
  );
}
```

By consuming the `GetStudents` method from an Angular component, you can retrieve the students list and display it in the view. This allows you to create a dynamic and interactive user interface that is updated in real-time with the data from the server.

**You can access and download an example project that I personally created to supplement this topic by clicking on this** [**link**](https://github.com/MbarkT3STO/JWTExample)**.**