# TypeScript Cheat Sheet

### CONTENTS

0. [TYPES](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#0-types)
1. [VARIABLES](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#1-variables)
2. [INTERFACES](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#2-interfaces)
3. [ENUMS](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#3-enums)
4. [FUNCTIONS AND GENERICS](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#4-functions-and-generics)
5. [TYPE ALIASES](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#5-type-aliases)
6. [SET UP](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#6-set-up)
7. [CONFIGURATION - tsconfig.json](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#7-configuration---tsconfigjson)
#
### 0: TYPES

#### A: Primitive (JavaScript):

- String
- Boolean
- Number
- BigInt
- Null
- Undefined
- Symbol (as of ES6)

#### B: Object (JavaScript)

#### C: Gradual Typing in TypeScript

- Any (allows falling back to JavaScript's regular behaviour and should be a last resort)

#

### 1: VARIABLES

Variable names:

- are alphanumeric
- cannot contain spaces or special characters (except $ and \_)
- cannot begin with a digit

#### A: Declare Type and Value

```
const name: string = "Zan"
let age: number = "21" // obviously! Maybe this should also be const? Regardless, this is now going to throw an error.
```

#### B: Declare Type but _not_ Value

```
const name: string;
let age: number;
```

#### C: Declare Value but _not_ Type (Inferred Typing)

```
const name = "Zan";
let age = 21;
```

The compiler will find the first usage of the variable in the code, determine what type it was originally set to, and then treat the variable as if it were explicitly declared to be that type.

#### D: Declare No Type or Value

```
let name; // data type is 'any' and value is set to undefined by default
```

#### E: Casting a Variable as the Any Type

```
let age: number

age = "Twenty One" as any
```

#

### 2: INTERFACES

#### A: Set Interface for an Object

```
interface Person {
  name: string;
  age: number;
  city: string;
  greet: ()=>string;
}
```

#### B: Use Interface as Template for an Object

```
let customer: Person = {
  name: "Zan",
  age: 21,
  city: "Manchester",
  greet: ():string =>{return "Hi!"}
}
```

Interfaces are not converted to JS. They have zero runtime impact.

#### C: Set a Property on an Interface to Read Only

```
interface Person {
  readonly id: number;
  name: string;
  age: number;
  city: string;
  greet: ()=>string;
}

let customer: Person = {
  id: 01234567, // now that id is set, it cannot be changed or overwritten
  name: "Zan",
  age: 21,
  city: "Manchester",
  greet: ():string =>{return "Hi!"}
}
```

#### D: Unions

Used for if you want a value to be assigned more than one data type, but not all.

```
interface Person {
  readonly id: number
  name: string;
  age: number | string;
  city: string;
  greet: ()=>string;
}

let customer: Person = {
  id: 01234567,
  name: "Zan",
  age: "21", // will also accept the number 21 without quotes
  city: "Manchester",
  greet: ():string =>{return "Hi!"},
}
```

#### E: Literal Values

Using literal values can allow you to limit a property to have only a small number of options as a value.

This is done inline and is good if you have only a small number of options. If you have lots of options, you should use an Enum, instead (described below).

```
interface Person {
  readonly id: number;
  name: string;
  age: number | string;
  city: "Liverpool" | "Manchester" | "York" ;
  greet: ()=>string;
}
```

#### F: Interfaces and Arrays

To type an array:

```
const numberArray: number[] = [1, 2, 3];
const stringArray: string[] = ["one", "two", "three", "four", "five"];
const numberAndStringArray: (number | string)[] = [1, "two", "three", 4, 5]
```

Interfaces can change the key of an array to a string, instead of a number.

```
interface ages {
  [index: string]: number;

}

const agelist: ages;

agelist["Zan"] = 21; // it may not be true, but this is correct
agelist[0] = 21; // throws an error
```

#

### 3: ENUMS

Enums let you set a prescriptive number of responses as a type instead of accepting any string or number. This is much the same as using literal values, as in the example above, however, this will tidy away that data and allow you to keep a longer, reuseable list that is not inline.

```
enum LocalBranch {
  Liverpool = "Liverpool", // 0 - If a value is not provided, as it is here...
  Manchester = "Manchester", // 1 - TS will assign a number to each key starting at 0...
  Newcastle = "Newcastle", // 2 - It will increment by 1 each time...
  Sheffield = "Sheffield", // 3
  York = "York" // 4 - And it will return that number by default
}

interface Person {
  readonly id: number;
  name: string;
  age: number | string;
  city: LocalBranch; // this interface now uses the enum to define city from a specified list
  greet: ()=>string;
}

let customer: Person = {
  id: 01234567
  name: "Zan",
  age: 21,
  city: LocalBranch.Manchester, // This has a value of "Manchester" as defined above
  greet: ():string =>{return "Hi!"}
}

```

#

### 4: FUNCTIONS AND GENERICS

Generics decorate a component with a type syntax in such a way that it can describe a variety of types rather than only one.

#### A: The Problem

The below function returns an object identical to the one passed in, but it will not automatically have the same type:

```
function clone(source) {
  const serialised = JSON.stringify(source);
  return JSON.parse(serialised);
}
const cloned = clone(customer)
```

#### B: An Adequate Solution

Specifying that the output type should be the same as the input type will certainly work, but limit its use to only one type.

```
function clone(source: Person): Person {
  const serialised = JSON.stringify(source);
  return JSON.parse(serialised);
}
const cloned = clone(customer)
```

#### C: A Great Solution

Using a type variable "<T>", as below, tells TypeScript that the output type should be the same as the input type, but has the flexibility to work with any type.

```
function clone<T>(source: T): T {
  const serialised = JSON.stringify(source);
  return JSON.parse(serialised);
}
const cloned = clone(customer)
```

#### D: Additional Parameters

Specify additional parameters like this:

```
function clone<T, U>(source: T, options: U) {...etc.
```

**Can also be used on interfaces and classes!**

#### E: Extends
This accepts multiple generic type parameters, but also ensures that the return type at least matches the properties of the input. It doesn't literally derive from the first type; it only has to match it, and then add any other properties.
  
In this example, `Contact` and `UserContact` are totally separate, but happen to contain some of the same properties:
```
interface Contact {
  id: number;
  name: string;
}
  
interface UserContact {
  id: number;
  name: string;
  username: string;
}
  
function clone<T1, T2 extends T1>(source: T1): T2 {
  return Object.apply({}, source);
}  
  
const a: Contact = {id: 123, name: "Homer Simpson" };
const b: clone<Contact, UserContact>(a)
```

### 5: TYPE ALIASES
Using type aliases, two interfaces can be combined into a type that contains the information from both.

```
interface Contact {
  id: number;
  name: ContactName;
  birthDate?: Date;
  status?: ContactStatus;
}

interface Address {
  line1: string;
  line2: string;
  province: string;
  region: string;
  postalCode: string;
}
  
```

Enums add more code to your final output. Type aliases can also be used to cut that down. This is a another alternative to [literal values](https://github.com/ZanClifton/coding-cheat-sheets/blob/main/cheatsheets/typescript.md#e-literal-values), above. TypeScript will limit the inputs for you, and you don't have to reference the enum to see what they are.
```
enum Status {
    todo = "todo",
    inProgress = "in-progress",
    done = "done"
}
  
type Status = "todo" | "in-progress" | "done"

```
#
### 6: SET UP
[Official Installation Instructions](https://www.typescriptlang.org/download)
  
Install the TypeScript compiler globally and run it:
```
$ npm install -g typescript
$ tsc
```
Or locally for your project:
```
$ npm install typescript --save-dev
$ npx tsc
```
 
TypeScript files have the `.ts` extension. Running the compiler will create ```.js``` files next to your ```.ts``` files, although instructions for configuring this are included below.
  
#
### 7: CONFIGURATION - tsconfig.json

#### A: File Creation and Initial Set Up
In the root of your app, create a file called `tsconfig.json`.

Add the following content:

```
{
    "include": ["src/**/*"], // if not set, all files and folders in the project will be included by default
}
```

This is an instruction to include all files in the src folder and its subfolders within the scope of the project. If not set, it will include everything by default.

You can control which version of JS the compiler uses (currently defaults to ES3), and where it outputs to, by adding the following to your `tsconfig.json` file:

```
{
    "include": ["src/**/*"]",
    "compilerOptions": {
        "target": "ES6", // most modern browsers use EcmaScript 6, so by setting it to this you are making your app widely accessible
        "outDir": "build", // by default, TypeScript generates the .js file next to the .ts file, this outputs the .js file elsewhere
    }
}
```

#### B: JavaScript Files

Enable TypeScript to check JavaScript files that are being used alongside your TypeScript files by adding the following to your `tsconfig.json` file in the `compilerOptions` object:

```
"allowJs": true, // allows .js files to be compiled
"checkJs": true // reports errors in .js files in TS version 2.3 or later
```

#### C: Native APIs

Allow TypeScript to account for native JavaScript and dom APIs by adding them to the `compilerOptions` object, e.g.:

```
"lib": ["es2015.promise", "dom"]
```

Again, this is similar to using the 'any' type, as you're telling TypeScript these APIs will be available in your target environments.
  
#### D: Third Party APIs
  
A quick (but not that helpful) fix:
```
declare var Vue: any;
```

A much better option is to follow the following steps.
  
Many libraries include TypeScript metadata in their bundles. For these, you can use the library's NPM module.
  
If a library you want to use does not, you can go to the [NPM website](https://www.npmjs.com) and search ```@types``` along with the name of the library you're looking to use. [DefinitelyTyped](https://definitelytyped.org) maintain TypeScript metadata for almost all popular frameworks.
  
![An example of the installation command for lodash, "npm install --save @types/lodash"](https://user-images.githubusercontent.com/96394256/179187920-db272c3d-9e66-4667-9ad5-8af7c06179f3.png)

Copy the command to into the terminal and run it in the root folder of your app. 
#  

### 8: KEYOF

Where an object has explicit keys, using this creates a union type with those keys. In the example below, `keyof Person` creates a union type of "name" and "age"; other strings will not be allowed.
  
```
interface Person {
  name: string;
  age: number;
}

function getValue(source: Person, property: keyof Person) {
  return source[property];
}
  
let person = {
  name: "Zan",
  age: 21
};
  
getValue(person, "name"); // if "name" is misspelled (e.g. "nmae" or "naem") TypeScript lets you know!

```
Better yet, the function above can be refactored into a generic function, as below:
```
function getValue<T>(source: T, property: keyof T) {
  return source[property];
}
```
The `keyof` operator can also be used to constrain generic types: 
```
function getValue<T, U extends keyof T>(source: T, property: U) {
  return source[property];
}
```
#
### 9: TYPEOF
```
function toContact(nameOrContact: string | Contact): Contact {
  if (typeof nameOrContact === "object")       
    return {
      id: nameOrContact.id,
      name: nameOrContact.name,
      status: nameOrContact.status
    }
  }
  else {
    return {
      id: 0,
      name: nameOrContact,
      status: "active"
    }
  }
}
```
#  
### 10: INDEXED ACCESS TYPES
```
interface Contact {
  id: number;
  name: string;
  status: ContactStatus;
  address: Address;
}

type Awesome = Contact["address"]["postalCode"]

interface ContactEvent {
  contactId: Contact["id"]; // explicit link to Contact interface, and id key
}
```
  
<!-- ```
interface Address {
    street: string;
    province: string;
    postalCode: string;
}
  
interface Contact {
    id: number;
    name: string;
    status: ContactStatus;
    address: Address;
}

type Awesome = Contact["address"]["postalCode"]

function getValue<T, U extends keyof T>(source: T, propertyName: U) {
    return source[propertyName];
}
``` -->
  
#
### 11: RECORDS

![Type 'number' is not assignable to type 'string'](https://user-images.githubusercontent.com/96394256/183680885-991f3cab-30b3-46e6-a7a6-48ac663d7a22.png)

This is not an issue with the 'number' key (which is a valid string), but with the type of the value. You can fix this by adding the value types you are using to the type definition, with a union.
  
  
![Screenshot 2022-08-09 at 15 53 42](https://user-images.githubusercontent.com/96394256/183682436-40dd98d2-915b-4653-9ef3-9a426a045119.png)

