# Modules in Angular

One of the core concepts in Angular is the use of modules. In this article, we will explore what modules are and how they are used in Angular.

## **What is a Module in Angular?**

In Angular, a module is a class with the `@NgModule` decorator. It is a container for a group of related components, directives, pipes, and services. A module helps organize an application by grouping related code together and separating it from unrelated code.

Modules in Angular can be divided into two types:

1. Feature modules: These are modules that contain a group of components, directives, pipes, and services that are related to a specific feature in the application. For example, a feature module could contain all the code related to displaying and editing user profiles.
    
2. Root module: This is the main module in an Angular application. It is the entry point for the application and it typically contains the root component and the bootstrap array. The root module is responsible for bootstrapping the application and it is typically the only module that is imported in the `main.ts` file.
    

## **How to Create a Module in Angular?**

To create a module in Angular, you first need to import the `NgModule` decorator from the `@angular/core` module. Then, you can define a class and decorate it with `@NgModule`.

Here is an example of a simple module in Angular:

```typescript
import { NgModule } from '@angular/core';

@NgModule({
  declarations: [],
  imports: [],
  exports: [],
  providers: []
})
export class MyModule { }
```

The `@NgModule` decorator takes an object with the following properties:

* `declarations`: An array of components, directives, and pipes that are part of this module.
    
* `imports`: An array of modules that this module depends on. These modules can be feature modules or other Angular libraries.
    
* `exports`: An array of components, directives, and pipes that should be available to modules that import this module.
    
* `providers`: An array of services that should be available to the rest of the application.
    

## **How to Use a Module in Angular?**

To use a module in Angular, you need to import it in the module that depends on it. For example, if you have a module called `MyModule` and you want to use it in the root module of your application, you can do the following:

```typescript
import { NgModule } from '@angular/core';
import { MyModule } from './my.module';

@NgModule({
  declarations: [],
  imports: [
    MyModule
  ],
  exports: [],
  providers: []
})
export class AppModule { }
```

Then, in your root component, you can use the components, directives, and pipes from `MyModule`.

## Real-World Example

Imagine you are building an e-commerce application that has a feature for displaying and managing products. In this case, you could create a feature module called `ProductModule` that contains all the code related to the product feature.

The `ProductModule` could have components for displaying a list of products, viewing a single product, and adding or editing a product. It could also have a service for fetching product data from an API and a pipe for formatting product prices.

To use the `ProductModule` in your application, you would import it in the root module and add it to the `imports` array. Then, in the root component, you could use the components and services from the `ProductModule` to display and manage products.

By organizing the product feature in a separate module, you can easily reuse it in other parts of the application and keep the code organized and maintainable.

First, we would create the `ProductModule`:

```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProductListComponent } from './product-list/product-list.component';
import { ProductDetailComponent } from './product-detail/product-detail.component';
import { ProductFormComponent } from './product-form/product-form.component';
import { ProductService } from './product.service';
import { PricePipe } from './price.pipe';

@NgModule({
  declarations: [
    ProductListComponent,
    ProductDetailComponent,
    ProductFormComponent,
    PricePipe
  ],
  imports: [
    CommonModule
  ],
  exports: [
    ProductListComponent,
    ProductDetailComponent,
    ProductFormComponent
  ],
  providers: [
    ProductService
  ]
})
export class ProductModule { }
```

Then, we would import the `ProductModule` in the root module and add it to the `imports` array:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ProductModule } from './product/product.module';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    ProductModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Finally, we could use the components and services from the `ProductModule` in the root component:

```typescript
import { Component } from '@angular/core';
import { ProductService } from './product/product.service';
import { Product } from './product/product';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'e-commerce';
  products: Product[];

  constructor(private productService: ProductService) { }

  ngOnInit() {
    this.getProducts();
  }

  getProducts() {
    this.productService.getProducts().subscribe(products => this.products = products);
  }
}
```

This is just a simple example of how you could use modules in Angular to organize and structure your application. You can use modules to group any kind of related code together and make it available to other parts of the application.

## **Conclusion**

Modules are a crucial concept in Angular that help organize and structure your application. They allow you to group related code together and make it available to other parts of the application. By creating and using modules, you can build a scalable and maintainable Angular application.