# Code style

## Naming

### Things that contains booleans

If 
* your variable contains `true`/`false` 
* your function(method) returns `true`/`false`

name it, using words like: `is`, `has`, `includes`, `some`, `every`, etc.

Examples:

```javascript
// BAD
const stringContainsOnlyNumbers = /^\d+$/.test('123');

// GOOD
const isDigitsOnly = /^\d+$/.test('123');
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
When you write code that hasn't concrete context, prefer 
names for variables which explains own destination

```javascript
// BAD: too pithy
const quantity = 10;

// GOOD
const registeredUsersQuantity = 10;  
```


### Function(method) name should begin from a verb

```javascript
// BAD
const namesOfUsersGet = array => array.map(({name}) => name);

// GOOD
const getUsersNames = array => array.map(({name}) => name);
```


### Lists and dictionaries names
For lists of items prefer plural entity name:

```javascript
// BAD
const arrayOfUsers = [{name: 'John'}, {name: 'Jane'}];

// GOOD
const users = [{name: 'John'}, {name: 'Jane'}]
```

For dictionaries prefer plural entity name with key, which you use for access for value:

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