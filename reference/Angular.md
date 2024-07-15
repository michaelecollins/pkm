---
date: July 14, 2024
epoch: 1721004770
aliases: #synonyms
tags:
---

# Angular

- [Angular](#angular)
  - [Angular Basics](#angular-basics)
    - [A Conceptual Overview of Angular](#a-conceptual-overview-of-angular)
      - [Angular Modules](#angular-modules)
    - [Angular \& Typescript](#angular--typescript)
  - [Angular Templates](#angular-templates)
    - [Angular Interpolation](#angular-interpolation)
    - [Angular Directives](#angular-directives)
      - [Structural Directives](#structural-directives)
        - [\*ngIf](#ngif)
        - [\*ngFor](#ngfor)
    - [Binding](#binding)
      - [Data Binding](#data-binding)
      - [Attribute Binding](#attribute-binding)
      - [Event Binding](#event-binding)
    - [Data Formatting](#data-formatting)
      - [Angular Pipes](#angular-pipes)
    - [Safe Navigation Operator](#safe-navigation-operator)
  - [Angular Components](#angular-components)
    - [Creating Components](#creating-components)
      - [Component Decorators](#component-decorators)
      - [Application Prefix](#application-prefix)
      - [Component Lifecycle Hooks](#component-lifecycle-hooks)
      - [Implementing a Hooks](#implementing-a-hooks)
      - [Inline Templates and Styles](#inline-templates-and-styles)
    - [Styling Components](#styling-components)
      - [CSS Encapsulation](#css-encapsulation)
      - [The ngClass Directive](#the-ngclass-directive)
      - [The ngStyle Directive](#the-ngstyle-directive)
    - [Compnent Communication](#compnent-communication)
      - [Communicating with Child Components](#communicating-with-child-components)
      - [Communicating with Parent Components](#communicating-with-parent-components)
  - [Angular Services](#angular-services)
    - [Injecting a Service into a Component](#injecting-a-service-into-a-component)
    - [Encapsulating Business Logic in a Service](#encapsulating-business-logic-in-a-service)
  - [HTTP Requests with Angular](#http-requests-with-angular)
    - [Observables](#observables)
  - [Routing \& Navigation](#routing--navigation)
  - [Angular Forms](#angular-forms)
    - [Template-driven Forms](#template-driven-forms)
    - [Template Variables](#template-variables)
    - [Working with Multiple Validators](#working-with-multiple-validators)
  - [Testing Angular Applications](#testing-angular-applications)
    - [Unit Testing](#unit-testing)
    - [End-to-End Testing](#end-to-end-testing)
    - [Executing Tests](#executing-tests)
    - [Modifying Unit Tests](#modifying-unit-tests)

## Angular Basics

### A Conceptual Overview of Angular

An angular application is primarily built by building components and services.

  1. **Components**: Define the UI.
     1. Contain HTML and TS
  2. **Services**: Contain business logic

Within an angular application, there is always a top-level Root component. The application can be routed to another component based on a url and that may subsequently be made from various sub components.

#### Angular Modules

A way to group components and services so that they can be loaded independently from each other.

Components must be declared within a module before they can be used.

### Angular & Typescript

Angular uses Typescript by default.

## Angular Templates

### Angular Interpolation

THe process of adding expressions into HTML which is then interpreted and rendered the result into the HTML by Angular.

Using interpolation:

```html
<h1>2 + 2 = {{2 + 2}}</h1>
```

Outputs:

```html
<h1>2 + 2 = 4</h1>
```

### Angular Directives

A collection of special operations specific to Angular.

#### Structural Directives

Directives that change the structure of html by adding or removing html elements. They are signified by prepending `*` to their name.

##### *ngIf

Conditionally show information in the html template.

```html
<ul>
    <li class="product-item" *ngFor="let product of products">
        <div *ngIf="!!product.name" class="product-name">{{product.name}}</div>
    </li>
</ul>
```

##### *ngFor

Repeats elements for a certain condition

```html
<ul>
    <li class="product-item" *ngFor="let product of products">
        <div class="product-name">{{product.name}}</div>
    </li>
</ul>
```

### Binding

Brackets:

- Square Brackets bind in the direction from the component to the template.
- Brackets are used to bind to expressions.

Parenthesis:

- Parenthesis bind in the direction from the template to the component.
- Parenthesis are used to bind to statements.

#### Data Binding

```typescript
// product.model.ts
export interface IProduct {
    id: number;
    name: string;
    description: string;
}

// catalog.component.ts
import { Component } from '@angular/core';

@Component({
    selector: 'catalog-foo',
    templateUrl: './catalog.component.html',
    styleUrls: ['./catalog.component.css']
})
export class Catalog {
    product: IProduct;

    constructor() {
        this.product = {
            id: 2,
            name: "a product",
            description: "the next big thing"
        }
    }
}
```

```html
<!-- catalog.component.html -->
<div class="name">{{product.name}}</div> // bind product name to html
```

#### Attribute Binding

Allows you to bind a property to an _expression_.

```html
<!-- catalog.component.html -->
<img src="..." alt="{{product.name}}" /> // using interpolation
<img src="..." [alt]="product.name" /> // using attribute binding
```

This represents a one-way binding. Square brackets create a binding only in the direction from the component class to the html attribute. Consider the following:

```html
<input type="text" [value]="product.name">
```

If the data changed in the component it _would_ change the value of the input box. However, when a user updated the value of the input box, that change would not be reflected in the components.

A property can also be bound to the return value of a function in the component.

```html
<!-- catalog.component.html -->
<img [src]="getImageUrl(product)" [alt]="product.name" /> // using attribute binding
```

```typescript
// catalog.component.ts
@Component({
    // ...
})
export class Catalog {
    product: IProduct;

    constructor() {
        // ...
    }

    getImageUrl(product: IProduct) {
        return '/dir/to/images/' + product.name;
    }
}
```

#### Event Binding

To bind to an html event which will update the component, use the `()` syntax. Typically, html events are prefixed with `on`, but in Angular, this is omitted. Instead, it would look something like:

```html
<div class="filters">
    <a class="button" (click)="filter='ft'">full-time</a>
    <a class="button" (click)="filter='pt'">part-time</a>
    <a class="button" (click)="filter='temp'">temporary</a>
</div>

<ul>
    <li class="product-item" *ngFor="let product of getFilteredProducts()">
        <div class="product-name">{{product.name}}</div>
    </li>
</ul>
```

```typescript
// catalog.component.ts
@Component({
    // ...
})
export class Catalog {
    product: IProduct;
    filter: string; // click handler sets filter value

    constructor() {
        this.product = {
            // ...
        }
    }

    getFilteredProducts() {
        // filter products
    }
}
```

### Data Formatting

#### Angular Pipes

A special syntax that easily allow formatting of data in html templates. There are a handful of pipes. Pipes can also accept data as an argument. An example of a built-in pipe is the currency pipe:

```html
<div class="price-usd">{{ product.price | currency: }}</div>
<div class="price-gbp">{{ product.price | currency:'GBP' }}</div>
```

### Safe Navigation Operator

Helps guard against null in html template. This is done by using the `null conditional operator` like in .net, `?.`.

```html
<ul>
    <li class="product-item" *ngFor="let product of getFilteredProducts()">
        <div class="product-name">{{product?.name}}</div>
    </li>
</ul>
```

## Angular Components

### Creating Components

You can easily create components by using the Angular CLI:

```typescript
ng generate component foo

// produces:
foo.component.css
foo.component.html
foo.component.spec.ts
foo.component.ts
```

Within the `foo.component.ts` file:

```typescript
import { Component } from '@angular/core';

@Component({ // component decorator
    selector: 'app-foo', // the html tag name you use to display this component in other components
    templateUrl: './foo.component.html',
    styleUrls: ['./foo.component.css']
})
export class Foo {
    // define properties, methods, data, and business logic here
}
```

#### Component Decorators

A Javascript concept which enables you to add metadata to a class or other object.

#### Application Prefix

Prefixes are intended to prevent collisions with any libraries you import. It should ideally be unique to your application. The prefix can be changed in the `angular.json` file so any future generated components use the correct prefix.

Consider the previous component:

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-foo', // "app" is the previx for this component.
    templateUrl: './foo.component.html',
    styleUrls: ['./foo.component.css']
})
export class Foo {

}
```

#### Component Lifecycle Hooks

Every component in Angular has a lifecycle. And that lifecycle is defined by a series of events that occur throughout the lifetime of a component. Some occur only once, while other occur multiple times during a component's lifetime.

| Once | During Change |
| ---- | ------------- |
| `OnInit` | `OnChanges` |
| `AfterContentInit` | `DoCheck` |
| `AfterViewInit` | `AfterContentChecked` |
| `OnDestroy` | `AfterViewChecked` |

The most common hooks used are:

- `OnChanges // if data changes`
- `OnInit    // fetch data`
- `OnDestroy // cleanup`

#### Implementing a Hooks

1. Import the event from angular
2. Implement the event on the class

```typescript
import { Component, OnInit } from '@angular/core'; // import event handler

@Component({
    selector: 'app-foo',
    templateUrl: './foo.component.html',
    styleUrls: ['./foo.component.css']
})
export class Foo implements OnInit { // implement event handler
    constructor() {}

    ngOnInit(): void { // note: prefixed with `ng`

    }
}
```

#### Inline Templates and Styles

It is possible to define inline html and css when you have very small components. To do this, pass `template` and `styles` into the component instead of urls for these properties.

However, this is not typically recommended as it can quickly become unmanageable.

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'app-foo', // "app" is the previx for this component.
    template: `
        <p class="red">
            Inline template!
        </p>
    `,
    styles: `
        .red {
            color: red;
        }
    `
})
export class Foo {

}
```

### Styling Components

**Global Styles** located in `styles.css`.

#### CSS Encapsulation

Any style directives applied within a component's `css` class only get applied to that component.

#### The ngClass Directive

If you want to apply styling to a component, there are a couple ways you can do that. One option is to use attribute binding:

```css
.strikethrough {
    text-decoration: line-through;
}
```

```html
<div [class.strikethrough]="product.cost > 0">
```

But if there are multiple classes, this becomes unwieldly. The solution is to use `ngClass`.

```html
<div [ngClass]="{ strikethrough: product.cost > 0 }">
<div [ngClass]="getProductCostClasses(product)">
```

#### The ngStyle Directive

Similar to how you can bind classes to an element, you can also bind styles to an element.

```html
<div [ngStyle]="{ 'color': 'red' }">
<div [ngStyle]="product.cost > 0 ? 'red' : ''">
<div [ngStyle]="getProductCostStyles(product)">
```

### Compnent Communication

#### Communicating with Child Components

```html
<!-- catalog.component.html -->
<ul class="products">
    <li class="product-item" *ngFor="let product of getProducts()">
        <app-product-details>
            <!-- passing product to product details component -->
            [product]="product"
        </app-product-details>
    </li>
</ul>
```

```html
<!-- product-details.component.html -->
<div class="product">
    <div class="product-details">
        <!-- ... -->
    </div>
</div>
```

```typescript
// product-details.component.ts
import { Component, Input } from 'angular/core';

@Component({
    selector: 'app-product-details',
    templateUrl: './product-details.component.html',
    styleUrls: './product-details.component.css'
})
export class ProductDetailsComponent {
    @Input() product: IProduct; // member of data will be injected into.

    addToCart() {

    }
}
```

#### Communicating with Parent Components

Example: In the product catalog example, each product is represented by a `product-details` component, including a button to add that item to their cart. However, the cart is not a member of the product component. It is a member of the parent component. So how is this information sent to the parent component?

```typescript
// product-details.component.ts
import { Component, Input, EventEmitter, Output } from 'angular/core';

@Component({
    selector: 'app-product-details',
    templateUrl: './product-details.component.html',
    styleUrls: './product-details.component.css'
})
export class ProductDetailsComponent {
    @Input() product: IProduct;
    @Output() buy = new EventEmitter() // register buy event

    buyButtonClicked(product: IProduct) {
        this.buy.emit(); // raise the event
    }
}
```

```html
<!-- catalog.component.html -->
<ul class="products">
    <li class="product-item" *ngFor="let product of getProducts()">
        <app-product-details>
            [product]="product"
            (buy)="addToCart(product)" <!-- register event listener-->
        </app-product-details>
    </li>
</ul>
```

```typescript
// catalog.component.ts
import { Component } from 'angular/core';

@Component({
    // ...
})
export class CatalogComponent {
    products: IProduct[];

    addToCart(product: IProduct) { // invoked from component html on (buy)
        // add to cart
    }
}
```

## Angular Services

### Injecting a Service into a Component

### Encapsulating Business Logic in a Service

## HTTP Requests with Angular

### Observables

## Routing & Navigation

## Angular Forms

### Template-driven Forms

### Template Variables

### Working with Multiple Validators

## Testing Angular Applications

### Unit Testing

### End-to-End Testing

### Executing Tests

### Modifying Unit Tests
