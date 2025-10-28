#oops
## 1. Core Idea

> **OOP = Organizing code around objects (classes) that contain both data (properties) and behavior (methods).**

Used heavily in **NestJS**, since everything (controllers, services, modules, guards) is a **class**.

---
## 2. Classes & Objects

A **class** defines a blueprint; an **object** (instance) is created from it.  

```ts

class UserService {

  createUser(name: string) {

    console.log(`User ${name} created`)

  }

}

  

const service = new UserService()

service.createUser("Anuj")

```


---
##  3. Constructor

- Runs automatically when an object is created.
- Used to **initialize data or inject dependencies**.

```ts

class UserService {

  private users: string[]

  

  constructor() {

    this.users = [] // initialize

  }

}

```

In **NestJS**, constructors are where **Dependency Injection (DI)** happens:

```ts

constructor(private readonly courseService: CourseService) {}

```

---
## 4. Dependency Injection (DI)

> A class receives its dependencies **from outside**, rather than creating them itself.

###  Without DI (tight coupling)

```ts

class UserService {

  private db = new DatabaseService() // directly creates dependency

}

```
### With DI (loose coupling)

```ts

class UserService {

  constructor(private readonly db: DatabaseService) {} // injected

}

```

**Why DI helps:**

- **Loose coupling:** swap implementations easily  
- **Modular:** each class has one job  
- **Reusable:** same class can work in many contexts  
- **Testable:** can inject mock/fake dependencies in tests  
  
---
## 5. Access Modifiers

Control **who can access** class members.

* **`public`** (default)
    * **Access:** anywhere
    * **Example:** `public name: string`
* **`private`**
    * **Access:** only inside the class
    * **Example:** `private password: string`
* **`protected`**
    * **Access:** inside the class + any subclasses
    * **Example:** `protected logPrefix = '[Base]'`
* **`readonly`**
    * **Access:** can be read but not reassigned
    * **Example:** `readonly id: number`
### Example

```ts
class Example {

  public name = "Anuj"

  private secret = "1234"

  protected role = "admin"

  readonly version = 1.0

  

  show() { console.log(this.secret) } // ✅ allowed here

}

```

---
## 6. Encapsulation

> Hide internal details; expose only what’s needed.

```ts

class BankAccount {

  private balance = 0


  deposit(amount: number) {

    this.balance += amount

  }

  getBalance() {

    return this.balance

  }

}

```


---
## 7. Inheritance

> A class **extends** another to reuse or override logic.

```ts
class BaseService {

  log(msg: string) { console.log(msg) }

}

class UserService extends BaseService {

  createUser() {

    this.log("Creating user...") // inherited method

  }

}

```
### Overriding & `super`

```ts

class CustomService extends BaseService {

  log(msg: string) {

    super.log("Custom: " + msg) // call parent version

  }

}

```

---
## 8. Abstract Classes

> Define **structure and shared logic**, but cannot be instantiated directly.

```ts

abstract class DatabaseService {

  abstract connect(): void

}

  

class MongoService extends DatabaseService {

  connect() { console.log("Connected to MongoDB") }

}

```

---
## 9. Interfaces

> Define a **contract (shape)** for data or classes — no implementation.

```ts
interface User {

  id: number

  name: string

}

function printUser(user: User) {

  console.log(user.name)

}

```

---
## 11. Polymorphism

> Different classes share the same interface or base type.

```ts
interface Database { connect(): void }

  

class MongoDB implements Database { connect() { console.log("MongoDB") } }

class Postgres implements Database { connect() { console.log("Postgres") } }

  

function initDB(db: Database) {

  db.connect()

}

```


---
## 12. Asynchronous Behavior

Most real Nest methods are async:  

```ts
async getUser(id: number) {

  return await this.userRepo.findOneById(id)

}

```

---
## 13. Decorators (TypeScript)

> Special functions that add **metadata or behavior** to classes, methods, or properties.

Examples in Nest:

```ts

@Controller('user')

export class UserController {

  constructor(private readonly userService: UserService) {}

  

  @Get()

  getAllUsers() { return this.userService.findAll() }

}

  

@Injectable() // marks class as provider for DI

export class UserService {}

```

---
