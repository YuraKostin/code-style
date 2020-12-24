# VanillaJS code style

## Naming

### Things that contain booleans

If 
* your variable contains `true`/`false` 
* your function(method) returns `true`/`false`

name it, using words like: `is`, `has`, `includes`, `some`, `every`, etc.

Examples:

```javascript
// BAD
const stringContainsOnlyNumbers = value => /^\d+$/.test(String(value));

// GOOD
const isDigitsOnly = value => /^\d+$/.test(String(value));
```

```javascript
// BAD
const validateThatBasketHasNoItems = basket => basket.items.length === 0;


// GOOD
const isEmptyBasket = basket => basket.items.length === 0;
```


### Avoid extra context in function body

When you have a function that already has context in its name - don't
bring the context in function body.

Examples:

```javascript
// BAD
function createUser({firstName, lastName, age}) {
    const userFullName = `${lastName} ${firstName}`;
    const userBirthYear = new Date(new Date() - age * 365 * 24 * 60 * 60 * 1000);
    
    return {
        age,
        birthYear: userBirthYear,
        firstName,
        fullName: userFullName,
        lastName,
    } 
}

// GOOD
function createUser({firstName, lastName, age}) {
    const fullName = `${lastName} ${firstName}`;
    const birthYear = new Date(new Date() - age * 365 * 24 * 60 * 60 * 1000);
    
    return {
        age,
        birthYear,
        firstName,
        fullName,
        lastName,
    } 
}
```

```javascript
// BAD
function getUsersQuantity() {
    let usersQuantity = 0;
  
    // calculations
  
    return usersQuantity;
}

// GOOD
function getUsersQuantity() {
    let quantity = 0;
  
    // calculations
  
    return quantity;
}
```

### Avoid long names

```javascript
// BAD: too much detailed
const whereUserWasOnPreviousHolidays = 'London';

// NORMAL: better
const previousHolidaysLocation = 'London';
  
// GOOD: using popular word reductions (ex: previous - prev, calculate - calc , accumulator - acc, etc...)
const prevHolidaysLocation = 'London';
```

### Avoid short names without context
When you write code that hasn't got a concrete context, prefer 
names for variables which explain own destination

```javascript
// BAD: too pithy
const quantity = 10;

// GOOD
const registeredUsersQuantity = 10;  
```


### Function(method) name should begin with a verb

```javascript
// BAD
const namesOfUsersGet = array => array.map(({name}) => name);

// GOOD
const getUsersNames = array => array.map(({name}) => name);
```


### Lists and dictionaries names
For lists of items prefer a plural entity name:

```javascript
// BAD
const arrayOfUsers = [{name: 'John'}, {name: 'Jane'}];

// GOOD
const users = [{name: 'John'}, {name: 'Jane'}]
```

For dictionaries prefer a plural entity name with key, which you use for access for value:

```javascript
// BAD
const dictionaryOfUsersPerId = {
    U2591: {name: 'John'},
    U5821: {name: 'Jane'},
};

// GOOD
const usersById = {
    U2591: {name: 'John'},
    U5821: {name: 'Jane'},
};
```

## Use 'get' word for function that returns value
There are such many words that can be used when you get something from a function:
* create
* produce
* build
* make
* compute
* combine
* calculate

but in the end we are definitely getting some value back.

So choose one word, that your team will use. It seems 'get' is the best option.

```javascript
// BAD
const produceEmptyObject = () => ({});
const calculateAnnualIncome = data => {...};
const combineStrings = (strings, glue) => strings.join(glue);

// GOOD
const getEmptyObject = () => ({});
const getAnnualIncome = data => {...};
const getConcatenatedStrings = (strings, glue) => strings.join(glue); // Savvy? =)
```

Obviously it is not a law, just recommendation, 
because we have words which describe function destination much better.

```javascript
// BAD
const getConcatenatedStrings = (strings, glue) => strings.join(glue);

// GOOD
const concatStrings = (strings, glue) => strings.join(glue);
const concatArrays = arrays => arrays.reduce((acc, array) => acc.concat(array), []);

// Or just multy type concat
const concat = (arg1, arg2...) => {...};
```


## Constants
Use UPPER_SNAKE_CASE for constants
```javascript
// BAD
const daysInAWeek = 7;
const someConstantValue = 12391;

// GOOD
const DAYS_IN_A_WEEK = 7;
const SOME_CONSTANT_VALUE = 12391;
```

## Pure functions

Prefer pure functions instead of impure when you can.

Pure function is a function that:
* has no side effects
* has same output for same input

```javascript
// BAD: impure
var a = 10;
  
function sum(b) {
    return a + b;
}
  
sum(10) // 20
a = 20;
  
sum(10) // 30


// GOOD: pure
var a = 1000;
  
function sum(a, b) {
    return a + b;
}
  
sum(5, 15); // 20
  
a = 123123;
  
sum(5, 15) // 20
```


## Magic numbers

Use obvious names for any numbers, that you may have in your programs.

```javascript
// BAD
const getMessageByStatusCode = statusCode => ({
    [404]: 'Sorry, but this page does not exists',
    [403]: 'Sorry, but you don\'t have enough permissions to see this page',
    [undefined]: 'Sorry, but something went wrong, please try again later' 
});

// GOOD
// http-codes.js
export const NOT_FOUND = 404;
export const FORBIDDEN = 403;

// example-utils.js
import {NOT_FOUND, FORBIDDEN} from '/path/to/http-code.js';

const getMessageByStatusCode = statusCode => ({
    [NOT_FOUND]: 'Sorry, but this page does not exists',
    [FORBIDDEN]: 'Sorry, but you don\'t have enough permissions to see this page',
    [undefined]: 'Sorry, but something went wrong, please try again later' 
});
```

It could look like some overhead, but most of your magic numbers could be needed in different parts of programs.

```javascript
// BAD
if (user.age >= 18 && user.country === 'Russia') {
    sellAlcohol(user);
} else if (user.age >= 21 && user.country === 'USA') {
    sellAlcohol(user);
} else {
    alert('You are too much young to buy alcohol');
}

// GOOD
const LEGAL_AGE_IN_RUSSIA = 18;
const LEGAL_AGE_IN_USA = 21;

if (user.age >= LEGAL_AGE_IN_RUSSIA && user.country === 'Russia') {
    sellAlcohol(user);
} else if (user.age >= LEGAL_AGE_IN_USA && user.country === 'USA') {
    sellAlcohol(user);
} else {
    alert('You are too much young to buy alcohol');
}

// or
const LEGAL_AGE_IN_RUSSIA = 18;
const LEGAL_AGE_IN_USA = 21;
const legalAgeByCountry = ({
    Russia: LEGAL_AGE_IN_RUSSIA,
    USA: LEGAL_AGE_IN_USA
})[user.country];

if (user.age >= legalAgeByCountry) {
    sellAlcohol(user);
} else {
    alert('You are too much young to buy alcohol');
}
```


## Small patterns

### If conditions

#### Prefer result of expression when it is possible (when result of expression is `boolean`)
```javascript
// BAD
let isMoreThanOneUserOnline;

if (usersOnline.length > 1) {
    isMoreThanOneUserOnline = true
} else {
    isMoreThanOneUserOnline = false;
}


// GOOD
const isMoreThanOneUserOnline = usersOnline.length > 1;
```

```javascript
// BAD
function isEmptyArray(array) {
    if (array.length === 0) {
        return true;
    } else {
        return false;
    }   
}

// GOOD
const isEmptyArray = array => array.length === 0;
```

#### Reduce if/else to if, when you don't need one of condition parts actually

```javascript
// BAD
if (count <= 10) {
    // do nothing
} else {
    alert('Done!');
}

// GOOD
if (count > 10) {
    alert('Done!');
}
```

```javascript
// BAD
if (blackListNames.includes(user.name)) {
    // do nothing
} else {
    alert('You have not been found in blacklist. Cheers!');
}

// GOOD
if (!blackListNames.includes(user.name)) {
    alert('You have not been found in blacklist. Cheers!');
}
```

> Attention: don't inverse expressions, when you will use both parts of condition

```javascript
// BAD
if (!blackListNames.includes(user.name)) {
    alert('You have not been found in blacklist. Cheers!');
} else {
    alert('You have been found in blacklist. You will be redirected!')
}

// GOOD
if (blackListNames.includes(user.name)) {
    alert('You have been found in blacklist. You will be redirected!')
} else {
    alert('You have not been found in blacklist. Cheers!');
}
```

#### Decisions based on concrete values

Use dictionaries, when you actually can map one value to another:

```javascript
// BAD
let message = '';

if (userName === 'Jane') {
    message = 'I love you, Jane';
} else if (userName === 'John') {
    message = 'You owe me, man!';
} else if (userName === 'James') {
    message = 'Did you see Voldemort?';
}

// GOOD
const messageByName = {
    Jane: 'I love you, Jane',
    John: 'You owe me, man!',
    James: 'Did you see Voldemort?',
    [undefined]: '',
};
const message = messageByName[user.name];
```

#### Early return

Don't do things, that couldn't be done by condition:

```javascript
// GOOD
function createUser(name, age) {
    if (name.length === 0 || age <= 0) {
        throw new Error('Cannot create user without `name` and `age`');
    }
    
    return {
        age,
        name,
    };
}
```

> This pattern couldn't be applied everywhere, but sometimes when you use it, code looks better
> because the pattern reduces code nesting. 

> With this pattern you could prevent function call with wrong arguments


## Single responsibility principle in functions: one function - one action

Prefer to write clean and simple functions, that do one thing.

```javascript
// BAD
const createDictOrPair = (key, value, isDict) => {
    if (isDict) {
        return {[key]: value};
    }
  
    return [key, value];
};


// GOOD

const dictOf = (key, value) => ({[key]: value});
  
const pairOf = (key, value) => [key, value];
```

> Almost every time when you see a boolean flag in arguments list, you possibly could split one function into two


## Imperative code vs Declarative code

Prefer declarative code style to imperative. People write code for people, not for computers.
So the cleaner and simpler code you write the easier it for support by other developers.

If you should choose between `for` loop which implements `map` and `map` function itself - prefer `map`.

```javascript
// Imperative
// BAD
const usersNames = [];

for (let i = 0; i < users.length; i++) {
    users.push(users[i].name);
}


// Declarative
// GOOD
const usersNames = users.map(({name}) => name);
```

> Attention: this concept is not about optimizations. For algorithms optimizations you may use 
>`for`, `while` and other imperative constructions.
> BTW, FP also has approaches for optimizing aka transducers.
