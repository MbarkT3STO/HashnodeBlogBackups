---
title: "ASP.NET Core: How to Apply Rate Limiting Algorithms?"
datePublished: Sun Jun 18 2023 18:38:15 GMT+0000 (Coordinated Universal Time)
cuid: clj1rtcbl000909lbgwt23dfd
slug: aspnet-core-how-to-apply-rate-limiting-algorithms
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1687110310015/ee83151b-4bca-4fc7-bf3a-bfc9d93c871c.webp
tags: web-development, security, net, aspnet-core

---

Rate limiting is a crucial technique to protect your [ASP.NET](http://ASP.NET) Core applications and APIs against malicious attacks and overuse. By restricting the number of requests allowed within a specific time window, rate limiting helps prevent Distributed Denial of Service (DDoS) attacks and API abuses. In [ASP.NET](http://ASP.NET) Core 7, you can leverage various rate limiting algorithms to implement this important security measure. In this article, we will explore how to apply rate limiting algorithms in [ASP.NET](http://ASP.NET) Core, including fixed window, sliding window, token bucket, and concurrency algorithms.

Lets assume that we have the following `Controller` :

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductController: ControllerBase 
{
	[HttpGet]
	public IActionResult Get()
	{
		return Ok("Hello World from Get()");
	}
}
```

With the controller in place, we can now proceed to apply the rate limiting algorithms.

## **Fixed Window Algorithm**

The fixed window algorithm allows a fixed number of requests within a specific time window. Any subsequent requests exceeding the limit are throttled. To configure the fixed window rate limiter in [ASP.NET](http://ASP.NET) Core 7, perform the following steps:

1. Open the `Program.cs` file in your [ASP.NET](http://ASP.NET) Core 7 project.
    
2. Add the following code snippet to configure the fixed window rate limiter:
    

```csharp
builder.Services.AddRateLimiter(options =>
{
	options.RejectionStatusCode = 429;
	options.AddFixedWindowLimiter(policyName: "fixed", options =>
	{
		options.PermitLimit = 3;
		options.Window = TimeSpan.FromMinutes(1);
	});
});
```

In the above code, `PermitLimit` represents the maximum number of requests allowed within the time window specified by `Window`.

## **Sliding Window Algorithm**

Similar to the fixed window algorithm, the sliding window algorithm allows a fixed number of requests per time window. However, the sliding window divides the time window into segments, with the window sliding by one segment at each interval. To configure the sliding window rate limiter in [ASP.NET](http://ASP.NET) Core 7, follow these steps:

1. Open the `Program.cs` file in your [ASP.NET](http://ASP.NET) Core 7 project.
    
2. Add the following code snippet to configure the sliding window rate limiter:
    

```csharp
builder.Services.AddRateLimiter(options =>
{
	options.RejectionStatusCode = 429;
	options.AddSlidingWindowLimiter(policyName: "sliding", options =>
	{
		options.PermitLimit = 3;
		options.Window = TimeSpan.FromMinutes(1);
		options.SegmentsPerWindow = 1;
	});
});
```

In the code snippet above, `SegmentsPerWindow` determines the number of segments the window is divided into. Requests are tracked and limited within each segment, allowing more flexibility compared to the fixed window algorithm.

### Details

The code snippet you demonstrates how to configure and add the sliding window rate limiter in [ASP.NET](http://ASP.NET) Core 7 using the `AddRateLimiter` method. Let's break it down step by step:

1. `options.RejectionStatusCode = 429;`: This sets the HTTP status code to be returned when a request exceeds the rate limit. In this case, it is set to 429, which stands for "Too Many Requests."
    
2. `options.AddSlidingWindowLimiter(policyName: "sliding", options => { ... })`: This line adds a sliding window rate limiter policy with the name "sliding." The `AddSlidingWindowLimiter` method is used to configure the sliding window rate limiter options.
    
3. `options.PermitLimit = 3;`: This sets the maximum number of requests allowed within the sliding window. In this example, it is set to 3, meaning that only three requests are allowed within the specified time window.
    
4. `options.Window = TimeSpan.FromMinutes(1);`: This defines the duration of the sliding window. It is set to 1 minute using the `TimeSpan.FromMinutes` method. Requests will be counted and limited within this time frame.
    
5. `options.SegmentsPerWindow = 1;`: This specifies the number of segments the sliding window is divided into. In this case, it is set to 1, meaning that the sliding window is not further divided into smaller segments.
    

By using the above configuration, the sliding window rate limiter will allow a maximum of 3 requests within a 1-minute window. If the limit is exceeded, subsequent requests will be rejected or throttled based on the configured rejection status code (429 in this case).

### Segments in details

In the context of rate limiting with sliding window algorithm, "segments" refer to dividing the time window into smaller intervals. Each segment represents a portion of the overall time window.

When using the sliding window rate limiter, the time window is divided into segments to track and enforce the rate limit. The number of segments determines the granularity of rate limiting within the window.

For example, if you have a time window of 1 minute and specify 2 segments, it means that the time window is divided into two equal intervals of 30 seconds each. The rate limit is applied separately within each segment, allowing a certain number of requests per segment.

The purpose of dividing the time window into segments is to provide more flexibility in rate limiting. It allows for more frequent resets within the time window, which can be useful in scenarios where you want to control the rate of requests more precisely.

By adjusting the number of segments, you can control how frequently the sliding window moves and resets the rate limit. A smaller number of segments means the window slides less frequently, while a larger number of segments results in more frequent sliding.

In the code snippet, `options.SegmentsPerWindow = 1;` indicates that the sliding window is not further divided into smaller segments. This means that the rate limit is applied uniformly across the entire time window without any additional segmentation.

## **Token Bucket Algorithm**

The token bucket algorithm assigns tokens to each request and allows processing only if tokens are available. Tokens are replenished at a fixed rate. To configure the token bucket rate limiter in [ASP.NET](http://ASP.NET) Core 7, perform the following steps:

1. Open the `Program.cs` file in your [ASP.NET](http://ASP.NET) Core 7 project.
    
2. Add the following code snippet to configure the token bucket rate limiter:
    

```csharp
builder.Services.AddRateLimiter(options =>
{
	options.RejectionStatusCode = 429;
	options.AddTokenBucketLimiter(policyName: "tokenbucket", options =>
	{
		options.TokenLimit = 3;;
		options.TokensPerPeriod = 3;
		options.ReplenishmentPeriod = TimeSpan.FromMinutes(1);
	});
});
```

The `TokenLimit` represents the maximum number of tokens available at any given time. `ReplenishmentPeriod` sets the duration between token refills, and `TokensPerPeriod` determines the number of tokens added in each replenishment.

### Details

The code snippet demonstrates how to configure and add a Token Bucket limiter using the `AddRateLimiter` method. Let's break down each component and understand their functionality:

* `options.TokenLimit = 3;` defines the maximum number of tokens (requests) that the Token Bucket can hold at any given time. In this case, the Token Bucket has a limit of 3 tokens.
    
* `options.TokensPerPeriod = 3;` specifies the number of tokens that are replenished in the Token Bucket per replenishment period. The replenishment period indicates the time it takes for the Token Bucket to refill. Here, 3 tokens are added to the Token Bucket every replenishment period.
    
* `options.ReplenishmentPeriod = TimeSpan.FromMinutes(1);` sets the duration of the replenishment period for the Token Bucket. In this example, the replenishment period is set to 1 minute. It determines how often the Token Bucket refills with tokens.
    

The Token Bucket algorithm works by assigning a token to each incoming request. If there are tokens available in the Token Bucket, the request is allowed and the token is consumed. If the Token Bucket is empty, indicating that the rate limit has been reached, the request is either rejected or throttled.

With the configuration above, the Token Bucket limiter allows a maximum of 3 requests (tokens) within the replenishment period of 1 minute. Once the token limit is reached, any additional requests will be subject to rate limiting.

## **Concurrency Algorithm**

The concurrency rate limiter restricts the number of simultaneous requests allowed. This algorithm limits the concurrent execution of requests rather than the total requests within a time window. To configure the concurrency rate limiter in [ASP.NET](http://ASP.NET) Core 7, follow these steps:

1. Open the `Program.cs` file in your [ASP.NET](http://ASP.NET) Core 7 project.
    
2. Add the following code snippet to configure the concurrency rate limiter:
    

```csharp
builder.Services.AddRateLimiter(options =>
{
	options.RejectionStatusCode = 429;
	options.AddConcurrencyLimiter(policyName: "concurrency", options =>
	{
		options.PermitLimit = 2;
		options.QueueLimit = 2;
	});
});
```

The `PermitLimit` represents the maximum number of simultaneous (in the same time) requests allowed, while `QueueLimit` sets the maximum number of requests that can be queued.

### Details

The code snippet demonstrates how to configure and add a Concurrency limiter using the `AddRateLimiter` method. Let's break down each component and understand their functionality:

* `options.PermitLimit = 2;` specifies the maximum number of requests that are allowed to execute concurrently. In this case, the Concurrency limiter allows a maximum of 2 concurrent requests.
    
* `options.QueueLimit = 2;` defines the maximum number of requests that can be queued when the concurrency limit is reached. If the number of concurrent requests exceeds the permit limit, additional requests will be queued. Here, the queue limit is set to 2, meaning that a maximum of 2 requests can be queued.
    

The Concurrency limiter helps control the number of requests that can be executed concurrently in an application. If the number of concurrent requests reaches the permit limit, additional requests will be either rejected or queued based on the specified configuration.

With the configuration above, the Concurrency limiter allows a maximum of 2 concurrent requests, and if the limit is reached, up to 2 additional requests can be queued. Any requests beyond the permit and queue limits will be subject to rate limiting.

## **Configuring Rate Limiting Middleware**

To enable the rate limiting middleware in your [ASP.NET](http://ASP.NET) Core 7 application, before the `app.Run();` add the following code snippet

```csharp
app.UseRateLimiter();
```

## **Applying the Rate Limiting on the Controllers**

To apply rate limiting on a controller, follow these steps:

1. Open the controller file where you want to apply rate limiting.
    
2. Locate the controller class declaration.
    
3. Add the `[EnableRateLimiting("PolicyName")]` attribute above the class declaration, replacing `"PolicyName"` with the actual name of the rate limiting policy you want to apply.
    

Here's an example of how the code might look:

```csharp
[EnableRateLimiting("concurrency")] // <<<<<<<< Here we apply the rate limiting policy
[ApiController]
[Route("api/[controller]")]
public class ProductController: ControllerBase 
{
	
	[HttpGet]
	public IActionResult Get()
	{
		return Ok("Hello World from Get()");
	}
}
```

In the example above, the `EnableRateLimiting` attribute is applied to the `ProductController` class, specifying the `"concurrency"` rate limiting policy. This means that any requests to the controller will be subject to the rate limiting rules defined in the `"concurrency"` policy.