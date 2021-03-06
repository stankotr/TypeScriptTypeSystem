![TypeScript Logo](https://cloud.githubusercontent.com/assets/3449303/18765110/8c5c603e-8114-11e6-9166-554b0face27b.png)

# PRIMER GitPitch

####  Deconstructing TypeScript's Type System

#####  Spencer Schneidenbach

@schneidenbach

---

Twitter @schneidenbach

## Slides plus more at

typescript.schneids.net

---

###  What is TypeScript?

![TypeScript Logo](https://cloud.githubusercontent.com/assets/3449303/18765110/8c5c603e-8114-11e6-9166-554b0face27b.png)

--- 

###  Why TypeScript?

Future of JavaScript, Today

---

###  Why TypeScript?

Better Tooling

---

###  Why TypeScript?

It's JavaScript That Scales

---

###  Best Thing about TypeScript

> It doesn't try to not be JavaScript - it just tries to make JavaScript better

?

---

###  Best Thing about TypeScript

> It doesn't try to not be JavaScript - it just tries to make JavaScript better

Me

---

###  Major Companies Are Using TypeScript

Google  

---

![Standards](https://imgs.xkcd.com/comics/standards.png)

---

###  Major Companies Are Using TypeScript

[Slack](https://slack.engineering/typescript-at-slack-a81307fa288d)

---

###  What TypeScript Isn't

"C# All The Things"

---

###  What TypeScript Isn't

> **TypeScript is manifestly not about turning JavaScript into C#.** You’ll never be best of breed in one ecosystem by attempting to retrofit another ecosystem.

---

> **TypeScript is manifestly not about turning JavaScript into C#.** You’ll never be best of breed in one ecosystem by attempting to retrofit another ecosystem.

Anders Heljsberg

![Anders Heljsberg](assets/anders.jpg)

---

###  Everyone Talks About

async/await  
classes  
arrow functions  
basic type usage

---

![TypeScript Logo](https://cloud.githubusercontent.com/assets/3449303/18765110/8c5c603e-8114-11e6-9166-554b0face27b.png)

Those are great reasons to use TypeScript

---

![TypeScript Logo](https://cloud.githubusercontent.com/assets/3449303/18765110/8c5c603e-8114-11e6-9166-554b0face27b.png)

Equally good - TypeScript's awesome type system

---

###  Let's Dive In

---

###  Let's Talk Basics

`number`, `string`, `boolean`  
`null`, `undefined`  
`array`
---

###  Implicit Types

Variables and functions will be given types by TypeScript where possible

```typescript
let aNumber = 42;   //aNumber is inferred to be type 'number'
```

If you mix types, TypeScript will show an error

```typescript
let aNumber = 42;
aNumber = "forty two";  //Error: aNumber is of type number
```

---

###  Explicit Types

You can add type annotations to your variables

```typescript
let aNumber: number = 42;
```

Explicitly marks variable as a type  
Useful when you don't know value ahead of time

---

###  Explicit Types Useful for Gradual Refactoring

`any` types

```typescript
let anAmbiguousType: any = 42;

anAmbiguousType = "a string";      //valid
anAmbiguousType = function() {};   //valid
anAmbiguousType = 123;             //valid
```

---

###  Functions

In JavaScript, it's a free-for-all

```typescript
function printNameAndAge(name, age) {
    console.log(`Hello ${name}!`);
    console.log(`You are ${age} years old.`);
}

printNameAndAge("Spencer", 30);
printNameAndAge();                              //totally valid
printNameAndAge("Jeff", "Jon", "Jay", "old");   //totally valid
```

---

###  Functions

In TypeScript, it's better enforced

```typescript
function printNameAndAge(name: string, age: number) {
    console.log(`Hello ${name}!`);
    console.log(`You are ${age} years old.`);
}

printNameAndAge("Spencer", 30);                 //ok
printNameAndAge();                              //error
printNameAndAge("Jeff", "Jon", "Jay", "old");   //def nope
```

---

###  Functions

In addition to parameters...

```typescript
function printNameAndAge(name: string, age: number) : void {
    console.log(`Hello ${name}!`);
    console.log(`You are ${age} years old.`);
}

```

You can add type annotations to functions to explicitly set a return type

---

###  Functions

This is compile-time enforced

```typescript
function operate(num1: number, num2: number, op: string): number {
    if (op === "add") {
        return num1 + num2;
    }
    //ERROR: return missing
}

```

---

###  Interfaces

The types you will use the most  
They are similar, but not the same as .NET interfaces!

---

###  Here's an interface

```typescript
interface Person {
    firstName: string;
    lastName; string;
}

```

---

###  Interfaces define structure

The "shape" of the object is what counts

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function addPerson(newPerson: Person) {
    //do some stuff
}

addPerson({
    firstName: "John",
    lastName: "Lackey"
});     //totally valid

```

---

###  Interfaces define structure

The "shape" of the object is what counts

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function addPerson(newPerson: Person) {
    //do some stuff
}

addPerson({
    firstName: "Matt"

})      //fails

```

---

###  Duck typing

If it quacks like a duck, walks like a duck, and looks like a duck, it's a duck

---

###  Duck typing

![PHP? No thanks!](assets/duck.jpg)

---

###  Optional properties

```typescript
interface Person {
    firstName: string;
    lastName?: string;
}

```

---

###  Optional properties

```typescript
interface Person {
    firstName: string;
    lastName?: string;
}

function addPerson(newPerson: Person) {
    //do some stuff
}

addPerson({
    firstName: "Matt"
})      //valid!

```

---

###  But that's not all

> “Interfaces are capable of describing the wide range of shapes that JavaScript objects can take”

TS docs

---

###  Function types

```typescript
interface Logger {
    (message: string, severity: string): void;
}

```

---

###  Function types

```typescript
interface Logger {
    (message: string, severity: string): void;
}

let aLogger: Logger = (message, severity) => {
    //log to console?
}

```

---

###  Function types

```typescript
interface jQuery {
    (selector: string): jQueryElement;
}
```
---

###  Function types

```typescript
interface jQuery {
    (selector: string): jQueryElement;

    ajax: (url: string, data) => Promise;
}
```

---

###  Function types

```typescript
interface jQuery {
    (selector: string): jQueryElement;

    ajax: (url: string, data) => Promise;
    extend: (obj1, obj2) => object;
}
```

---

###  Index types

Specifies a type as "indexable"

```typescript
interface NumberDictionary {
    [index: string]: number;
}

```

The index type must be a number or string  
Useful for enforcing the return type of an index

---

###  We'll revisit index types

---

![Alton Brown](assets/alton.jpg)

---

###  Runtime impact of interfaces?

---

Interfaces don't compile down to anything

```typescript
////Person.ts
interface Person {
    firstName: string;
    lastName: string;
}

////Person.js
//nothing here!

```

Use them to give meaning to your code

---

###  Generics

Allows you to define reusable constraints on types

```typescript
let thePersons: Array<Person> = [];

thePersons.push({
    firstName: "John",
    lastName: "Lackey"
});

thePersons.push("John Lackey");     //error

```

---

###  Classes

Groups of data and actions, like C#/VB.NET classes

```typescript
class Animal {
    string: name;
    
    move() : void {
        console.log(`${this.name} moved.`);
    }
}

```

---

###  Classes

Classes can implement interfaces

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

class Employee implements Person {
    firstName: string;
    lastName: string;
    dateHired: Date;
}

```

---

###  Classes

Classes can inherit from other classes

```typescript
class SalesRepresentative extends Employee {
    salesCode: string;
}

```

---

###  Classes

Strongest use cases are inheritance and code reuse

---

![Overdone OO](assets/oo.png)

---

###  Enums

Enums are just like enums in C# and VB.NET

```typescript
enum Suit { 
    Heart,
    Spade,
    Diamond,
    Club
}

let aSuit: Suit = Suit.Club;

```

---

###  const Enums

Inlines use of the enum

```typescript
const enum Suit { 
    Heart,
    Spade,
    Diamond,
    Club
}

let aSuit: Suit = Suit.Club;

```

Compiles to:

```typescript
let aSuit = 3;

```

A little more performant

---

###  Enum tip

When using enums, start the enum with number 1

```typescript
enum Suit { 
    Heart = 1,
    Spade,
    Diamond,
    Club
}

```

This allows you to assert an enum value as truthy

---

###  String literal types

Ooh, new syntax

```typescript
type Suit = "Heart" | "Diamond" | "Spade" | "Club";

let aSuit: Suit = "Heart";
let anotherSuit: Suit = "Four"; //error!

```

---

As far as the `type` syntax goes...

---

![Alton Brown](assets/alton.jpg)

---

###  Enums vs string literals

I prefer string literals, but you might like enum

---

###  Enums vs string literals

Real world:

```csharp
////Person.cs
enum SecurityClearance { TopSecret, Lowest }

public class Person
{
    [JsonConverter(typeof(StringEnumConverter))]
    public SecurityClearance SecurityClearance { get; set; }
}
```

If you deserialize enums as string, use string literals  
Otherwise use enums

---

###  Tuples

Arrays are commonly used as tuples in JavaScript

```typescript
let numberPair = [123, "one hundred twenty three"];

```

---

###  Tuples

You can add type annotations to enforce the "tuple" type

```typescript
function printNumbers(aNumberTuple: [number, string]) {
    console.log(`${aNumberTuple[0]}` `${aNumberTuple[1]}`);
}

let numberPair = [123, "one hundred twenty three"];
printNumbers(numberPair);   //ok
printNumbers(["two", 2]);   //error

```

---

###  Type assertions
###  Type guards

But before we do...

---

Let's talk about `instanceof` vs `typeof` briefly

---

###  `typeof`

Returns a `string`  
Used to compare primitives

---

###  `typeof`

Code | Returns
---- | ------
`typeof "asdf"` | `"string"`
`typeof 123` | `"number"`
`typeof function() {}` | `"function"`
`typeof {}` | `"object"`
`typeof new Employee()` | `"object"`
`typeof undefined` | `"undefined"`
`typeof null` | `"object"`

---

###  `typeof`

TypeScript is contextually aware when you're in a `typeof` block

```typescript
function printLowerCase(arg: any) {
    if (typeof arg === "string") {
        console.log(arg.toLowerCase());
    } else {
        console.log(arg);
    }
}

```

---

###  `instanceof`

Inspects the prototype chain

```typescript
class Person {
    firstName: string;
}

let person = new Person();
console.log(person instanceof Person);  //true
console.log(person instanceof Date);    //false

```

---

###  Just one problem...

JavaScript lacks sophisticated runtime introspection  
You can't use `typeof` or `instanceof` to plain JS objects to compare to another object  
(Well, you can, but it wouldn't be very useful)

```typescript
let employee = new Employee("John");
let employeeStructurally = {
    firstName: "John"
};

typeof employee;                            //object
employee instanceof Employee;               //true
employeeStructurally instanceof Employee;   //false

```

---

###  Another problem...

Interfaces can't use `instanceof` because they don't exist in JS

```typescript
interface Person {
    firstName: string;
}

let personStructurally = {
    firstName: "John"
};

personStructurally instanceof Person;   //error!

```

---

###  Solution: type guards

We can add user-defined type guards like so:

```typescript
function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim !== undefined;
}

if (isFish(pet)) {
    pet.swim();
} else {
    pet.fly();
}

```

This gives the TS compiler enough information to assure that a given type is what it says it is

---

We'll come back to type guards

---

In other words...

---

![Alton Brown again](assets/alton.jpg)

---

###  Union types

Marks a type as being either or

```typescript
let input: number | string;

input = "this";       //valid!
input = 123;          //valid!
input = {             //error!
    words: "ok"
}
```

---

###  Union types

```typescript
interface AjaxOptions {
    url: string;
    type: string;
}

function ajaxRequest(urlOrOptions: string | AjaxOptions) {}
```

---

###  Union types

```typescript
interface AjaxOptions {
    url: string;
    type: string;
}

function ajaxRequest(urlOrOptions: string | AjaxOptions) {
    if (typeof urlOrOptions === "string") {
        return $.get(urlOrOptions);
    } else {
        return $.ajax(urlOrOptions.url, {
            type: urlOrOptions.type
        });
    }
}

```
---

###  Intersection types

Ever seen this before?

```typescript
Object.assign(objectOne, objectTwo);
angular.extend(objectOne, objectTwo);
$.extend(objectOne, objectTwo);

```

Result is the same - `objectOne` now has all of the properties from `objectTwo`

---

###  Intersection types

Represents two or more types combined into one

```typescript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }
    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}

```

---

### Intersection types

```typescript
interface Dog { breed: string; }
interface Dinosaur { hasSharpTeeth: boolean; }

let dog: Dog = { breed: "Poodle" };
let velociraptor: Dinosaur = { hasSharpTeeth: true };

let abomination = extend(dog, velociraptor);

console.log(abomination.breed);         //valid!
console.log(abomination.hasSharpTeeth); //valid!
```

---

###  You can do some cool things

From TypeScript documentation: 

```typescript
type LinkedList<T> = T & { next: LinkedList<T> };

interface Person {
    name: string;
}

var people: LinkedList<Person>;
var s = people.name;
var s = people.next.name;
var s = people.next.next.name;
var s = people.next.next.next.name;
```

---

###  Type aliases

Incredibly useful for union and intersection types  
Defines types you can use over and over again

```typescript
//union type
type StringOrNumber = string | number;

//intersection type
type jAngular = typeof jQuery  
                & typeof angular
                & { isEvil: boolean };

```

---

###  Aliases are also useful for `strictNullChecks`

If compiler option `--strictNullChecks` is on...

```typescript
let aNumber: number = undefined;    //not allowed

type NumberOrUndefined = number | undefined;

let anotherNumber: NumberOrUndefined = undefined;  //ok
```

---

###  Type aliases vs interfaces

*Interface* | *Type Alias*
----------- | ------------
Can be used in extends or implements clause | Can't be used to extend other interfaces/classes
Can have multiple merged declarations | Can't have merged declarations

---

### Example of a merged declaration:

```typescript
interface Person {
    firstName: string;
}

//declared later
interface Person {
    lastName: string;
}
```

---

###  Type capture

You can capture the type of another variable:

```typescript
let animal = {
    name: "Cat",
    legs: 4
};

let capturesType: typeof animal = {
    //error unless you set name and legs
}
```

---

###  `keyof`

Used to capture the keys (or property names) of another type.

```typescript
let animal = {
    name: "Cat",
    legs: 4
};

let animalKey: keyof typeof animal;

animalKey = "name";     //ok!
animalKey = "legs":     //ok!
animalKey = "anythingElse";     //ERROR

```

---

###  Using `keyof` with indexed types

```typescript
interface Person {
    firstName: string;
    lastName: string;
    age: number;
}

type Descriptions<T> = {
    [P in keyof T]?: string;
}

let personDescriptions: Descriptions<Person> = {
    firstName: "The person's first name.",
    age: "How old the person is."
}

```

---

###  Create some interesting types

How about a type that takes the properties of another and makes them readonly?  

From the TypeScript docs:

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}

type ReadonlyPerson = Readonly<Person>;

```

---

###  Implementing pluck using `keyof` for type safety

Pluck is used to take properties from an object.

```typescript
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map(n => o[n]);
}

let person: Person = {
    firstName: 'Jarid',
    age: 35
};

pluck(person, ['firstName']);      // ok
pluck(person, ['age', 'unknown']); // error

```

---

###  Note the use of a generic constraint

Yes, TypeScript has those as well!

```typescript
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map(n => o[n]);
}

```

---

###  Discriminated Unions

Used to "tag" types with specific data. From the TypeScript docs:

```typescript
interface Square {
    kind: "square";
    size: number;
}
interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}
interface Circle {
    kind: "circle";
    radius: number;
}
type Shape = Circle | Rectangle | Square;
```

---

###  Now we can do stuff like this

```typescript
type Shape = Circle | Rectangle | Square;

function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        case "circle": return Math.PI * s.radius ** 2;
    }
}
```

TypeScript's compiler is aware of what type the object is after the `case` statement

---

###  `never` type

A type that can never be.  
`never` can only represent "impossible" types or `never` types.

```typescript
let cantExist: never;

cantExist = "something";    //error!
```

---

###  Use it with discriminated unions

Gives the TypeScript compiler exhaustive checking

```typescript
function assertNever(x: never): never {
    throw new Error("Unexpected object: " + x);
}

function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        case "circle": return Math.PI * s.radius ** 2;
        default: return assertNever(s); //error
    }
}

```

---

###  If we ever extend Shape, we'll be protected

```typescript
type Shape = Square | Rectangle | Circle | Triangle;
function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        case "circle": return Math.PI * s.radius ** 2;
        default: return assertNever(s);
    }
    //error - didn't handle Triangle!
}
```

---

###  Real world example

Let's wrap some things together

---

### The backend

Let's use ASP.NET as our example

```csharp
public class PersonPostRequest
{
    [Required]
    public string FirstName { get; set; }

    [Required]
    public string LastName { get; set; }
}
```

---

### The handler

```csharp
public PersonController : Controller
{
    public IActionResult Post(PersonPostRequest personPostRequest)
    {
        if (ModelState.IsValid)
        {
            //save!
            return Ok(new IdResponse { Id = newPerson.Id });
        }
        else
        {
            //try again
            return BadRequest(ModelState);
        }
    }
}

```

---

#### Your POST request can return either

`200 OK`

```json
{
    "id": 1234
}
```

`400 Bad Request`

```json
{
    "message": "The request is invalid.",
    "modelState": {
        "firstName": [
            "First name is required."
        ]
    }
}
```

---

###  Modeling the request in TypeScript

```typescript
interface PersonPostRequest {
    firstName: string;
    lastName: string;
}
```

---

###  Modeling responses in TypeScript

```typescript
interface PersonOkResponse {
    id: number;
}

//indexed type and keyof
interface BadRequestResponse<TRequest> {
    message: string;
    modelState: {
        [K in keyof TRequest]: string[];
    };
}
```

---

```typescript
interface PersonPostRequest { firstName: string; lastName: string; }

interface BadRequestResponse<TRequest> {
    message: string;
    modelState: {
        [K in keyof TRequest]?: string[];
    };
}

interface PersonOkResponse { id: number; }

//union type
type PersonResponse = 
    PersonOkResponse | BadRequestResponse<PersonPostRequest>
```

---

### Create your request method

```typescript
type PersonResponse = 
    PersonOkResponse | BadRequestResponse<PersonPostRequest>

function postPerson(request: PersonPostRequest) : Promise<PersonResponse> {
    return fetch("api/Persons", request)
        .then(data => data.json() as Promise<PersonResponse>);
}
```

---

###  Use a type guard to handle correct response

```typescript
//type guard
function isErrorResponse(response: PersonResponse) 
    : response is BadRequestResponse<PersonPostRequest> {
    return (response as BadRequestResponse<PersonPostRequest>).modelState !== undefined;
}

//type guard compile time awesomeness!
function handleResponse(response: PersonResponse) {
    if (isErrorResponse(response)) {
        console.log(response.modelState.firstName[0]);  //or whatever...
    } else {
        console.log(response.id);
    }
}
```

--- 

### That's the tip of the iceberg

---

### Getting Started

Greenfield vs brownfield

---

### More resources

typescript.schneids.net  
typescriptlang.org

---

## Thank you!

[@schneidenbach](https://twitter.com/schneidenbach)  
schneids.net
