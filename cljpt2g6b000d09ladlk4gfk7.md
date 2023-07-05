---
title: "Angular: Working with FormData"
datePublished: Wed Jul 05 2023 14:19:47 GMT+0000 (Coordinated Universal Time)
cuid: cljpt2g6b000d09ladlk4gfk7
slug: angular-working-with-formdata
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1688565803750/8fa8e918-0b37-4a72-a173-93b5d44c2956.png
tags: angular, aspnet-core

---

In this article, we will explore how to work with FormData in Angular when communicating with an [ASP.NET](http://ASP.NET) Core Web API. FormData is a JavaScript object that allows you to build and send data in a format compatible with HTML forms. It's particularly useful when you need to handle file uploads or send complex data structures.

## **Setting up the Backend**

First, let's set up our [ASP.NET](http://ASP.NET) Core Web API backend to receive and process the FormData. We will use a DTO (Data Transfer Object) class to define the structure of the data we expect to receive.

```csharp
public class MyDto
{
    public string Name { get; set; }
    public int Age { get; set; }
    public IFormFile File { get; set; }
}
```

In the above code snippet, we define a `MyDto` class with properties for the name, age, and a file. The `IFormFile` type represents an uploaded file.

Next, create an API endpoint to receive the FormData:

```csharp
[HttpPost]
public IActionResult UploadData([FromForm] MyDto myDto)
{
    // Process the data and perform necessary operations
    // ...
    return Ok();
}
```

Here, we define an `UploadData` action that accepts a `MyDto` object from the request body using the `[FromForm]` attribute.

## **Implementing the Frontend in Angular**

Now let's move on to the Angular frontend and see how we can work with FormData when making requests to our [ASP.NET](http://ASP.NET) Core Web API.

First, import the necessary Angular modules:

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';
```

Next, create a component and inject the `HttpClient` in its constructor:

```typescript
@Component({
  selector: 'app-upload',
  templateUrl: './upload.component.html',
  styleUrls: ['./upload.component.css']
})
export class UploadComponent {
  constructor(private http: HttpClient) { }
}
```

### **Uploading FormData**

To upload FormData, we'll need to create an HTML form and handle the submission using Angular.

In your template file (`upload.component.html`), add the following markup:

```xml
<form (ngSubmit)="uploadData($event)">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" [(ngModel)]="name" required>
  </div>
  <div>
    <label for="age">Age:</label>
    <input type="number" id="age" name="age" [(ngModel)]="age" required>
  </div>
  <div>
    <label for="file">File:</label>
    <input type="file" id="file" name="file" (change)="handleFileChange($event.target.files)">
  </div>
  <button type="submit">Upload</button>
</form>
```

In the above code, we define an HTML form with input fields for name, age, and file. We use Angular's `ngModel` to bind the input values to component properties.

In your component class (`upload.component.ts`), add the following methods:

```typescript
export class UploadComponent {
  name: string;
  age: number;
  file: File;

  constructor(private http: HttpClient) { }

  uploadData(event: Event): void {
    event.preventDefault();

    const formData = new FormData();
    formData.append('name', this.name);
    formData.append('age', this.age.toString());
    formData.append('file', this.file);

    this.http.post('/api/upload', formData).subscribe(() => {
      // Handle success
    }, (error) => {
      // Handle error
    });
  }

  handleFileChange(files: FileList): void {
    this.file = files.item(0);
  }
}
```

In the `uploadData` method, we create a new FormData object and append the input values to it. Then, we make a POST request to the `/api/upload` endpoint, passing the FormData as the request body.

The `handleFileChange` method is triggered when a file is selected. It assigns the selected file to the `file` property.