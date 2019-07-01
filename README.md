# Async - Callbacks, Promises, Async Await

> My simple tutorial asynchronous JavaScript we will cover callbacks, promises including promise.all as well as the async / await and fetch.

## What is a Callback()
Take a look for simple rendering example below:
```
/**
 * @author Jino
 * @email jinowilbertolacson@gmail.com
 * @create date 2019-07-01 13:30:53
 * @modify date 2019-07-01 13:30:53
 * @desc basic address book rendering with timeouts
 */

// calback.js
//Collection of address
const addressBook = [
    { name: 'Juan dela cruz', address: 'Tagas Daraga, Albay' },
    { name: 'Joey Marquez', address: 'Bogtong Legazpi City, Albay' }
  ];

//Iterate address to document with 1 second timeout
function getAddressBook() {
setTimeout(() => {
    let output = '';
    addressBook.forEach((addrbook, index) => {
    output += `<li>${addrbook.name}</li>`;
    });
    document.body.innerHTML = output;
}, 1000);
}

//Create new address and append to address array with 2 second timeout
function createNewAddress(newAddressBook) {
setTimeout(() => {
    addressBook.push(newAddressBook);
}, 2000);
}

//Call getAddressBook
getAddressBook();

//Call createNewAddress function and append to getAddressBook
createNewAddress({ name: 'Jino', address: 'Lacson' });
```


The output will be  :

* Juan dela cruz
* Joey Marquez


But why ***createNewAddress*** did not append `{ name: 'Jino', address: 'Lacson' }` to **addressBook** array?.

The answer is ***createNewAddress*** took `2 second longer` than ***getAddressBook*** display `has only 1 second` we can do anything beyond that point. 

Thats where `callback` comes in take a look for example.

<details>
<summary>Solution `callback.js`</summary>
```
// file: callback.js

/**
 * @author Jino
 * @email jinowilbertolacson@gmail.com
 * @create date 2019-07-01 13:30:53
 * @modify date 2019-07-01 13:30:53
 * @desc Simple demonstration with callback() function
 */

//Collection of address
const addressBook = [
  { name: 'Juan dela cruz', address: 'Tagas Daraga, Albay' },
  { name: 'Joey Marquez', address: 'Bogtong Legazpi City, Albay' }
];

//Iterate address to document with 1 second timeout
function getAddressBook() {
  setTimeout(() => {
    let output = '';
    addressBook.forEach((addrbook, index) => {
      output += `<li>${addrbook.name}</li>`;
    });
    document.body.innerHTML = output;
  }, 1000);
}

//Create new address and append to address array with 2 second timeout
function createNewAddress(newAddressBook, callback) {
  setTimeout(() => {
    addressBook.push(newAddressBook);
    callback();
  }, 2000);
}

//Call createNewAddress function and call getAddressBook
createNewAddress({ name: 'Jino', address: 'Lacson' }, getAddressBook);
```
</details>
The output will be:

* Juan dela cruz
* Joey Marquez
* Jino

Using **callback** it will append new `{ name: 'Jino', address: 'Lacson' }` address, even though
***createNewAddress*** has `2 second time` than ***getAdddressBook*** has only `1 second time` durations.




# Promise() , Promise.all, Async, Wait and Fetch

## Using promises.
Take a look to our simple code example we have `addressBook` which holds person informations and `getAddressBook` function that will output person's name.

```
// file: promises.js

/**
 * @author Jino
 * @email jinowilbertolacson@gmail.com
 * @create date 2019-07-01 13:33:57
 * @modify date 2019-07-01 13:33:57
 * @desc Simple promise code template
 * 
 */

// Collection of address
const addressBook = [
    { name: 'Juan dela cruz', address: 'Tagas Daraga, Albay' },
    { name: 'Joey Marquez', address: 'Bogtong Legazpi City, Albay' }
  ];

// Iterate addresses to document with 1 second timeout
function getAddressBook() {
  setTimeout(() => {
    let output = '';
    addressBook.forEach((addrbook, index) => {
      output += `<li>${addrbook.name}</li>`;
    });
    document.body.innerHTML = output;
  }, 1000);
}

/**
 * Create new Address that will return a promise wether it is resolve or a reject.
 * 
 * Promise takes two parameters specific:
 * resolve = for success promise
 * reject = for errors
 */
function createNewAddress(addrbook) {
    return new Promise((resolve, reject) => {
    setTimeout(() => {
        addressBook.push(addrbook);

        const error = false;

        if (!error) {
        resolve();
        } else {
        reject('Error: Something went wrong');
        }
    }, 2000);
    });
}
```

<details>
<summary>Simple `Promise`</summary>
```
/**
 * Create new Address and return a promise either resolve or reject
 */
createNewAddress({ name: 'Jino', address: 'Lacson' })
.then(getAddressBook)
.catch(err => console.log(err));
```
</details>

<details>
<summary>Using `async` and `await`</summary>
```
/**
 * Create new address using Async and Await
 */
async function init() {
    // Create new address book first
   await createNewAddress({ name: 'Jino', address: 'Lacson' });

   // .. then display addresses after creating new address
   getAddressBook();
}
//call init function
init();
```
</details>

<details>
<summary>Using `Promise.all`</summary>
```
// Promise.all
const promise1 = Promise.resolve('Hello World');
const promise2 = 10;
const promise3 = new Promise((resolve, reject) =>
  setTimeout(resolve, 2000, 'promise3')
);
const promise4 = fetch('https://jsonplaceholder.typicode.com/users').then(res =>
  res.json()
);
const promise5 = createNewAddress({ name: 'Jino', address: 'Lacson' })
.then(getAddressBook)
.catch(err => console.log(err));

Promise.all([promise1, promise2, promise3, promise4, promise5]).then(values =>
  console.log(values)
);
```
</details>

<details>
<summary>Example of using `async` `wait` and `fetch`</summary>
```
async function fetchUsers() {
    const res = await fetch('https://jsonplaceholder.typicode.com/users');
    const data = await res.json();//convert to json
    console.log(data);
}
//call fetchUsers()
fetchUsers();
```
</details>

