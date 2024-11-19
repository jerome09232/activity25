![1](https://cdn.hashnode.com/res/hashnode/image/upload/v1732018883951/e139cdd7-d2ef-4fa9-89b4-a640b1238bcd.jpeg?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp)


# Activity 25: SOLID PRINCIPLES in Angular
The acronym SOLID stands for:

S: Single Responsibility Principle

O: Open/Closed Principle

L: Liskov Substitution Principle

I: Interface Segregation Principle

D: Dependency Inversion Principle

In this article, we will explore each of these principles and how they can be applied to improve code structure, reusability, and maintainability in Angular.

## Single Responsibility Principle (SRP)

The Single Responsibility Principle (SRP) is a fundamental principle of software engineering which states that a class or module should have only one reason to change. In other words, it should have only one responsibility. Violating this principle can lead to code that is difficult to maintain and understand.

## Violation of SRP

Consider a component that displays a list of products and allows the user to filter them by category. Here’s an example of what the code might look like:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.service';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.css']
})
export class ProductListComponent implements OnInit {

  products: any[];
  categories: string[];
  selectedCategory: string;

  constructor(private productService: ProductService) { }

  ngOnInit() {
    this.products = this.productService.getProducts();
    this.categories = this.productService.getCategories();
  }

  filterByCategory(category: string) {
    this.selectedCategory = category;
    this.products = this.productService.getProductsByCategory(category);
  }

}
```
This component violates the SRP because it has two responsibilities: displaying the list of products and filtering them by category. This violates the “one reason to change” principle, as changes to the filtering behavior may affect the component’s rendering logic and vice versa.

To fix this violation, we can refactor the component into two separate components, each with its own responsibility.

## Fixing the Violation

First, we’ll create a new component that displays the list of products:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.service';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.css']
})
export class ProductListComponent implements OnInit {

  products: any[];

  constructor(private productService: ProductService) { }

  ngOnInit() {
    this.products = this.productService.getProducts();
  }

}
```
This component is now responsible only for displaying the list of products. We’ve removed the category filtering logic to a new component.

Next, we’ll create a new component that handles the category filtering:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.service';

@Component({
  selector: 'app-product-filter',
  templateUrl: './product-filter.component.html',
  styleUrls: ['./product-filter.component.css']
})
export class ProductFilterComponent implements OnInit {

  categories: string[];
  selectedCategory: string;

  constructor(private productService: ProductService) { }

  ngOnInit() {
    this.categories = this.productService.getCategories();
  }

  filterByCategory(category: string) {
    this.selectedCategory = category;
    this.productService.getProductsByCategory(category);
  }

}
 ``` 
This component is now responsible only for handling the category filtering. It updates the selected category and calls the getProductsByCategory() method of the ProductService to update the list of products.

Finally, we’ll update the app.component.html file to include both components:

<app-product-filter (categorySelected)="productList.filterByCategory($event)"></app-product-filter>
<app-product-list #productList></app-product-list>

The app-product-filter component emits a categorySelected event when a new category is selected. This event is handled by the app-product-list component, which calls its filterByCategory() method with the selected category.

By splitting the responsibilities into two separate components, we’ve successfully refactored our code to comply with the Single Responsibility Principle. Each component now has a single responsibility, making our code more modular and easier to maintain.

In addition to complying with the SRP, this refactoring also follows the Separation of Concerns principle, which advocates for dividing a software system into separate modules, each addressing a separate concern.

Adhering to the SRP is crucial for writing maintainable, extensible, and reusable code. By keeping each module’s responsibilities separated, we can avoid coupling and ensure that any changes to one module do not affect others unnecessarily.

## Open/Closed Principle (OCP)

The Open/Closed Principle states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. In other words, we should be able to add new functionality without modifying existing code. This principle helps in reducing code coupling, improving code scalability, and making the code more reusable.

OCP can be applied using inheritance and interfaces. For example, we can create a base component or service that contains common functionality and then create child components or services that inherit from the base component or service. We can also create interfaces for components or services that define a set of methods or properties that must be implemented.

## Violating the Open/Closed Principle

Consider, we have a component that displays a list of users. The users are retrieved and displayed in a table. The table has three columns: name, email, and phone number. We have created a UserListComponent to display this list. Initially, the component looks like this:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {
  users: any[];

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}

In the template file user-list.component.html, we have the following code:

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users">
      <td>{{ user.name }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phone }}</td>
    </tr>
  </tbody>
</table>
 ``` 
Everything works fine, and we can see the list of users displayed in a table with three columns.

Now, suppose we want to add a new feature to the application, which is to allow users to view additional details for each user. We want to display the user’s address, city, and state. We can simply modify the UserListComponent to add these columns to the table. Here’s how we can do it:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {
  users: any[];

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users">
      <td>{{ user.name }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phone }}</td>
      <td>{{ user.address }}</td>
      <td>{{ user.city }}</td>
      <td>{{ user.state }}</td>
    </tr>
  </tbody>
</table>
 ``` 
This code violates the OCP because we have modified the existing component to add new functionality. If we want to add more features to the application, we will have to keep modifying the UserListComponent, which makes it difficult to maintain and scale.

Fixing the violation

To fix the violation of the OCP, we need to separate the concerns of displaying the list of users and displaying the details of each user. We can create a new component called UserDetailComponent to display the additional details for each user. Here’s how we can do it:
 ``` typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-user-detail',
  template: `
    <div>
      <p><strong>Address:</strong> {{ user.address }}</p>
      <p><strong>City:</strong> {{ user.city }}</p>
      <p><strong>State:</strong> {{ user.state }}</p>
    </div>
  `
})
export class UserDetailComponent {
  @Input() user: any;
}
 ``` 
In the UserListComponent, we can now use the UserDetailComponent to display the additional details for each user. Here’s how we can do it:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {
  users: any[];

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users">
      <td>{{ user.name }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phone }}</td>
      <td>
        <button (click)="user.showDetails = !user.showDetails">
          {{ user.showDetails ? 'Hide Details' : 'Show Details' }}
        </button>
        <app-user-detail *ngIf="user.showDetails" [user]="user"></app-user-detail>
      </td>
    </tr>
  </tbody>
</table>
 ``` 
In this new implementation, the UserListComponent is responsible for displaying the list of users, while the UserDetailComponent is responsible for displaying the details of each user. The UserListComponent doesn’t need to be modified to add new functionality because the UserDetailComponent can be easily extended to display additional details.

## Liskov Substitution Principle (LSP)

The Liskov Substitution Principle (LSP) is a fundamental concept in software engineering that is essential for designing and implementing maintainable and extensible systems. LSP states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. Violating the LSP can lead to unexpected behavior and bugs.

## Violating LSP
 ``` typescript
class Animal {
  move() {
    console.log('I can move');
  }
}

class Bird extends Animal {
  fly() {
    console.log('I can fly');
  }
}

class Ostrich extends Animal {
  move() {
    console.log('I can move, but I cannot fly');
  }
}

function moveAnimal(animal: Animal) {
  animal.move();
}

const bird = new Bird();
moveAnimal(bird); // outputs "I can move"

const ostrich = new Ostrich();
moveAnimal(ostrich); // outputs "I can move, but I cannot fly"
 ```
In this example, Ostrich is a subclass of Animal that overrides the move() method to reflect the fact that ostriches cannot fly. However, when we pass an instance of Ostrich to the moveAnimal() function, we get unexpected behavior because the move() method of Ostrich does not behave the same way as the move() method of Animal. This violates the LSP because Ostrich is not a true substitute for Animal.

To fix the violation, we can modify the Ostrich class to behave more like Animal:
 ``` typescript
class Ostrich extends Animal {
  move() {
    console.log('I can move, but I cannot fly');
  }

  fly() {
    throw new Error('I cannot fly');
  }
}
 ``` 
In this updated implementation, we have added a fly() method to the Ostrich class that throws an error because ostriches cannot fly. This ensures that Ostrich behaves like Animal and can be used as a substitute for it without affecting the correctness of the program.

The Liskov Substitution Principle is an important principle in software engineering that helps us design maintainable and extensible systems. Violating the LSP can lead to unexpected behavior and bugs, but these violations can be fixed by ensuring that subclasses behave like their superclasses.

Interface Segregation Principle (ISP)

The Interface Segregation Principle (ISP) is a software development principle that states that a software component should not be forced to depend on interfaces that it does not use. In other words, clients should not be forced to depend on methods they do not use. This principle helps to reduce coupling between components and makes code more modular, flexible, and maintainable.

ISP can be violated when a component has a dependency on a service that provides more methods than the component needs. This can lead to unnecessary coupling between the component and the service, making it difficult to modify either component without affecting the other. Additionally, it can cause performance issues as the component may be forced to load unnecessary methods from the service.

To demonstrate this, let’s consider an example where a component needs to retrieve data from a service. The service provides several methods, including ones that are not required by the component.
 ``` typescript
interface DataService {
  getItems(): Observable<Item[]>;
  addItem(item: Item): Observable<Item>;
  updateItem(item: Item): Observable<Item>;
  deleteItem(id: number): Observable<boolean>;
}
 ``` 
The component only needs to retrieve items from the service, so it should only depend on the getItems method. However, if the component is written as follows:
 ``` typescript
class MyComponent {
  items: Item[];
  
  constructor(private dataService: DataService) {}
  
  ngOnInit() {
    this.dataService.getItems().subscribe(items => this.items = items);
  }
}
 ``` 
The component is depending on the DataService interface, which includes methods that are not used by the component. This can lead to unnecessary coupling and performance issues.

To fix this, we can create a new interface that only includes the getItems method and have the service implement this interface. The component can then depend on this new interface instead of the original DataService interface.
 ``` typescript
interface ItemsService {
  getItems(): Observable<Item[]>;
}

class DataService implements ItemsService {
  getItems() { ... }
  addItem(item: Item) { ... }
  updateItem(item: Item) { ... }
  deleteItem(id: number) { ... }
}

class MyComponent {
  items: Item[];
  
  constructor(private itemsService: ItemsService) {}
  
  ngOnInit() {
    this.itemsService.getItems().subscribe(items => this.items = items);
  }
}
 ``` 
Now, the component only depends on the ItemsService interface, which includes only the getItems method. This helps to reduce coupling and improves performance.

The Interface Segregation Principle (ISP) is an important software development principle that helps to reduce coupling between components and makes code more modular, flexible, and maintainable. Violating ISP can lead to unnecessary coupling and performance issues. To fix this, we can create new interfaces that include only the methods needed by a component and have services implement these interfaces instead of larger interfaces. This can help to improve code quality, reduce coupling, and improve performance.

Dependency Inversion Principle (DIP)

Dependency Inversion Principle (DIP) is an important principle of object-oriented design. It suggests that high-level modules should not depend on low-level modules, but both should depend on abstractions. The abstractions should not depend on the details, but the details should depend on the abstractions. This principle promotes loose coupling and high cohesion in software systems.

Violation of DIP

Let’s consider an example where we have a CustomerService that depends on a CustomerRepository to fetch customer data. The implementation of CustomerService looks like this:
 ``` typescript
import { CustomerRepository } from './customer.repository';

@Injectable()
export class CustomerService {
  constructor(private customerRepository: CustomerRepository) {}

  getCustomers(): Observable<Customer[]> {
    return this.customerRepository.getCustomers();
  }
}

The implementation of CustomerRepository looks like this:

@Injectable()
export class CustomerRepository {
  private customers: Customer[] = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Jack' }
  ];

  getCustomers(): Observable<Customer[]> {
    return of(this.customers);
  }
}
 ```
The CustomerService class depends on the CustomerRepository class, which violates the Dependency Inversion Principle. The CustomerService class is a high-level module, and the CustomerRepository class is a low-level module. The CustomerService class should not depend on the CustomerRepository class directly.

## Fixing Violation of DIP

To fix the violation of the Dependency Inversion Principle, we need to introduce an abstraction between the CustomerService class and the CustomerRepository class. We can introduce an interface ICustomerRepository to define the contract that the CustomerRepository class should implement. The implementation of CustomerService looks like this:
 ``` typescript
import { ICustomerRepository } from './customer.repository';

@Injectable()
export class CustomerService {
  constructor(private customerRepository: ICustomerRepository) {}

  getCustomers(): Observable<Customer[]> {
    return this.customerRepository.getCustomers();
  }
}

The implementation of ICustomerRepository looks like this:

export interface ICustomerRepository {
  getCustomers(): Observable<Customer[]>;
}

The implementation of CustomerRepository now implements the ICustomerRepository interface.

@Injectable()
export class CustomerRepository implements ICustomerRepository {
  private customers: Customer[] = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Jack' }
  ];

  getCustomers(): Observable<Customer[]> {
    return of(this.customers);
  }
}
 ```
Now, the CustomerService class depends on the ICustomerRepository interface, and the CustomerRepository class implements the ICustomerRepository interface. This way, the CustomerService class is decoupled from the CustomerRepository class, and both depend on the abstraction.The acronym SOLID stands for:

S: Single Responsibility Principle

O: Open/Closed Principle

L: Liskov Substitution Principle

I: Interface Segregation Principle

D: Dependency Inversion Principle

In this article, we will explore each of these principles and how they can be applied to improve code structure, reusability, and maintainability in Angular.

## Single Responsibility Principle (SRP)

The Single Responsibility Principle (SRP) is a fundamental principle of software engineering which states that a class or module should have only one reason to change. In other words, it should have only one responsibility. Violating this principle can lead to code that is difficult to maintain and understand.

## Violation of SRP

Consider a component that displays a list of products and allows the user to filter them by category. Here’s an example of what the code might look like:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.service';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.css']
})
export class ProductListComponent implements OnInit {

  products: any[];
  categories: string[];
  selectedCategory: string;

  constructor(private productService: ProductService) { }

  ngOnInit() {
    this.products = this.productService.getProducts();
    this.categories = this.productService.getCategories();
  }

  filterByCategory(category: string) {
    this.selectedCategory = category;
    this.products = this.productService.getProductsByCategory(category);
  }

}
 ``` 
This component violates the SRP because it has two responsibilities: displaying the list of products and filtering them by category. This violates the “one reason to change” principle, as changes to the filtering behavior may affect the component’s rendering logic and vice versa.

To fix this violation, we can refactor the component into two separate components, each with its own responsibility.

## Fixing the Violation

First, we’ll create a new component that displays the list of products:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.service';

@Component({
  selector: 'app-product-list',
  templateUrl: './product-list.component.html',
  styleUrls: ['./product-list.component.css']
})
export class ProductListComponent implements OnInit {

  products: any[];

  constructor(private productService: ProductService) { }

  ngOnInit() {
    this.products = this.productService.getProducts();
  }

}
 ``` 
This component is now responsible only for displaying the list of products. We’ve removed the category filtering logic to a new component.

Next, we’ll create a new component that handles the category filtering:

 ``` typescript
import { Component, OnInit } from '@angular/core';
import { ProductService } from './product.service';

@Component({
  selector: 'app-product-filter',
  templateUrl: './product-filter.component.html',
  styleUrls: ['./product-filter.component.css']
})
export class ProductFilterComponent implements OnInit {

  categories: string[];
  selectedCategory: string;

  constructor(private productService: ProductService) { }

  ngOnInit() {
    this.categories = this.productService.getCategories();
  }

  filterByCategory(category: string) {
    this.selectedCategory = category;
    this.productService.getProductsByCategory(category);
  }

}
 ``` 
This component is now responsible only for handling the category filtering. It updates the selected category and calls the getProductsByCategory() method of the ProductService to update the list of products.

Finally, we’ll update the app.component.html file to include both components:

<app-product-filter (categorySelected)="productList.filterByCategory($event)"></app-product-filter>
<app-product-list #productList></app-product-list>

The app-product-filter component emits a categorySelected event when a new category is selected. This event is handled by the app-product-list component, which calls its filterByCategory() method with the selected category.

By splitting the responsibilities into two separate components, we’ve successfully refactored our code to comply with the Single Responsibility Principle. Each component now has a single responsibility, making our code more modular and easier to maintain.

In addition to complying with the SRP, this refactoring also follows the Separation of Concerns principle, which advocates for dividing a software system into separate modules, each addressing a separate concern.

Adhering to the SRP is crucial for writing maintainable, extensible, and reusable code. By keeping each module’s responsibilities separated, we can avoid coupling and ensure that any changes to one module do not affect others unnecessarily.

## Open/Closed Principle (OCP)

The Open/Closed Principle states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. In other words, we should be able to add new functionality without modifying existing code. This principle helps in reducing code coupling, improving code scalability, and making the code more reusable.

OCP can be applied using inheritance and interfaces. For example, we can create a base component or service that contains common functionality and then create child components or services that inherit from the base component or service. We can also create interfaces for components or services that define a set of methods or properties that must be implemented.

## Violating the Open/Closed Principle

Consider, we have a component that displays a list of users. The users are retrieved and displayed in a table. The table has three columns: name, email, and phone number. We have created a UserListComponent to display this list. Initially, the component looks like this:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {
  users: any[];

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}

In the template file user-list.component.html, we have the following code:

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users">
      <td>{{ user.name }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phone }}</td>
    </tr>
  </tbody>
</table>
 ``` 
Everything works fine, and we can see the list of users displayed in a table with three columns.

Now, suppose we want to add a new feature to the application, which is to allow users to view additional details for each user. We want to display the user’s address, city, and state. We can simply modify the UserListComponent to add these columns to the table. Here’s how we can do it:
 ``` typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {
  users: any[];

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users">
      <td>{{ user.name }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phone }}</td>
      <td>{{ user.address }}</td>
      <td>{{ user.city }}</td>
      <td>{{ user.state }}</td>
    </tr>
  </tbody>
</table>
 ``` 
This code violates the OCP because we have modified the existing component to add new functionality. If we want to add more features to the application, we will have to keep modifying the UserListComponent, which makes it difficult to maintain and scale.

## Fixing the violation

To fix the violation of the OCP, we need to separate the concerns of displaying the list of users and displaying the details of each user. We can create a new component called UserDetailComponent to display the additional details for each user. Here’s how we can do it:
 ``` typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-user-detail',
  template: `
    <div>
      <p><strong>Address:</strong> {{ user.address }}</p>
      <p><strong>City:</strong> {{ user.city }}</p>
      <p><strong>State:</strong> {{ user.state }}</p>
    </div>
  `
})
export class UserDetailComponent {
  @Input() user: any;
}

In the UserListComponent, we can now use the UserDetailComponent to display the additional details for each user. Here’s how we can do it:

import { Component, OnInit } from '@angular/core';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit {
  users: any[];

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Phone</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users">
      <td>{{ user.name }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phone }}</td>
      <td>
        <button (click)="user.showDetails = !user.showDetails">
          {{ user.showDetails ? 'Hide Details' : 'Show Details' }}
        </button>
        <app-user-detail *ngIf="user.showDetails" [user]="user"></app-user-detail>
      </td>
    </tr>
  </tbody>
</table>
 ``` 
In this new implementation, the UserListComponent is responsible for displaying the list of users, while the UserDetailComponent is responsible for displaying the details of each user. The UserListComponent doesn’t need to be modified to add new functionality because the UserDetailComponent can be easily extended to display additional details.

## Liskov Substitution Principle (LSP)

The Liskov Substitution Principle (LSP) is a fundamental concept in software engineering that is essential for designing and implementing maintainable and extensible systems. LSP states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. Violating the LSP can lead to unexpected behavior and bugs.

## Violating LSP
 ``` typescript
class Animal {
  move() {
    console.log('I can move');
  }
}

class Bird extends Animal {
  fly() {
    console.log('I can fly');
  }
}

class Ostrich extends Animal {
  move() {
    console.log('I can move, but I cannot fly');
  }
}

function moveAnimal(animal: Animal) {
  animal.move();
}

const bird = new Bird();
moveAnimal(bird); // outputs "I can move"

const ostrich = new Ostrich();
moveAnimal(ostrich); // outputs "I can move, but I cannot fly"
 ``` 
In this example, Ostrich is a subclass of Animal that overrides the move() method to reflect the fact that ostriches cannot fly. However, when we pass an instance of Ostrich to the moveAnimal() function, we get unexpected behavior because the move() method of Ostrich does not behave the same way as the move() method of Animal. This violates the LSP because Ostrich is not a true substitute for Animal.

To fix the violation, we can modify the Ostrich class to behave more like Animal:
 ``` typescript
class Ostrich extends Animal {
  move() {
    console.log('I can move, but I cannot fly');
  }

  fly() {
    throw new Error('I cannot fly');
  }
}
 ``` 
In this updated implementation, we have added a fly() method to the Ostrich class that throws an error because ostriches cannot fly. This ensures that Ostrich behaves like Animal and can be used as a substitute for it without affecting the correctness of the program.

The Liskov Substitution Principle is an important principle in software engineering that helps us design maintainable and extensible systems. Violating the LSP can lead to unexpected behavior and bugs, but these violations can be fixed by ensuring that subclasses behave like their superclasses.

## Interface Segregation Principle (ISP)

The Interface Segregation Principle (ISP) is a software development principle that states that a software component should not be forced to depend on interfaces that it does not use. In other words, clients should not be forced to depend on methods they do not use. This principle helps to reduce coupling between components and makes code more modular, flexible, and maintainable.

ISP can be violated when a component has a dependency on a service that provides more methods than the component needs. This can lead to unnecessary coupling between the component and the service, making it difficult to modify either component without affecting the other. Additionally, it can cause performance issues as the component may be forced to load unnecessary methods from the service.

To demonstrate this, let’s consider an example where a component needs to retrieve data from a service. The service provides several methods, including ones that are not required by the component.
 ``` typescript
interface DataService {
  getItems(): Observable<Item[]>;
  addItem(item: Item): Observable<Item>;
  updateItem(item: Item): Observable<Item>;
  deleteItem(id: number): Observable<boolean>;
}

The component only needs to retrieve items from the service, so it should only depend on the getItems method. However, if the component is written as follows:

class MyComponent {
  items: Item[];
  
  constructor(private dataService: DataService) {}
  
  ngOnInit() {
    this.dataService.getItems().subscribe(items => this.items = items);
  }
}
 ```
The component is depending on the DataService interface, which includes methods that are not used by the component. This can lead to unnecessary coupling and performance issues.

To fix this, we can create a new interface that only includes the getItems method and have the service implement this interface. The component can then depend on this new interface instead of the original DataService interface.
 ``` typescript
interface ItemsService {
  getItems(): Observable<Item[]>;
}

class DataService implements ItemsService {
  getItems() { ... }
  addItem(item: Item) { ... }
  updateItem(item: Item) { ... }
  deleteItem(id: number) { ... }
}

class MyComponent {
  items: Item[];
  
  constructor(private itemsService: ItemsService) {}
  
  ngOnInit() {
    this.itemsService.getItems().subscribe(items => this.items = items);
  }
}
 ``` 
Now, the component only depends on the ItemsService interface, which includes only the getItems method. This helps to reduce coupling and improves performance.

The Interface Segregation Principle (ISP) is an important software development principle that helps to reduce coupling between components and makes code more modular, flexible, and maintainable. Violating ISP can lead to unnecessary coupling and performance issues. To fix this, we can create new interfaces that include only the methods needed by a component and have services implement these interfaces instead of larger interfaces. This can help to improve code quality, reduce coupling, and improve performance.

## Dependency Inversion Principle (DIP)

Dependency Inversion Principle (DIP) is an important principle of object-oriented design. It suggests that high-level modules should not depend on low-level modules, but both should depend on abstractions. The abstractions should not depend on the details, but the details should depend on the abstractions. This principle promotes loose coupling and high cohesion in software systems.

## Violation of DIP

Let’s consider an example where we have a CustomerService that depends on a CustomerRepository to fetch customer data. The implementation of CustomerService looks like this:
 ``` typescript
import { CustomerRepository } from './customer.repository';

@Injectable()
export class CustomerService {
  constructor(private customerRepository: CustomerRepository) {}

  getCustomers(): Observable<Customer[]> {
    return this.customerRepository.getCustomers();
  }
}

The implementation of CustomerRepository looks like this:

@Injectable()
export class CustomerRepository {
  private customers: Customer[] = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Jack' }
  ];

  getCustomers(): Observable<Customer[]> {
    return of(this.customers);
  }
}
 ``` 
The CustomerService class depends on the CustomerRepository class, which violates the Dependency Inversion Principle. The CustomerService class is a high-level module, and the CustomerRepository class is a low-level module. The CustomerService class should not depend on the CustomerRepository class directly.

## Fixing Violation of DIP

To fix the violation of the Dependency Inversion Principle, we need to introduce an abstraction between the CustomerService class and the CustomerRepository class. We can introduce an interface ICustomerRepository to define the contract that the CustomerRepository class should implement. The implementation of CustomerService looks like this:
 ``` typescript
import { ICustomerRepository } from './customer.repository';

@Injectable()
export class CustomerService {
  constructor(private customerRepository: ICustomerRepository) {}

  getCustomers(): Observable<Customer[]> {
    return this.customerRepository.getCustomers();
  }
}

The implementation of ICustomerRepository looks like this:

export interface ICustomerRepository {
  getCustomers(): Observable<Customer[]>;
}

The implementation of CustomerRepository now implements the ICustomerRepository interface.

@Injectable()
export class CustomerRepository implements ICustomerRepository {
  private customers: Customer[] = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Jack' }
  ];

  getCustomers(): Observable<Customer[]> {
    return of(this.customers);
  }
}
 ``` 
Now, the CustomerService class depends on the ICustomerRepository interface, and the CustomerRepository class implements the ICustomerRepository interface. This way, the CustomerService class is decoupled from the CustomerRepository class, and both depend on the abstraction.


##  Hashnode Link : https://jeromeberiso.hashnode.dev/activity-25-solid-principles-in-angular

