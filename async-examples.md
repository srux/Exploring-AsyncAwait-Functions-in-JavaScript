### Simple Example

```javascript
function scaryClown() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('Benny');
    }, 2000);
  });
}

async function msg() {
  const msg = await scaryClown();
  console.log('Message:', msg);
}

msg(); // Message: Benny <-- after 2 seconds

// await is a new operator used to wait for a promise to resolve or reject. It can only be used inside an async function.
```

### Multiple Steps
```javascript
function who() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('Benny');
    }, 200);
  });
}

function what() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('lurks');
    }, 300);
  });
}

function where() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('in the shadows');
    }, 500);
  });
}

async function msg() {
  const a = await who();
  const b = await what();
  const c = await where();

  console.log(`${ a } ${ b } ${ c }`);
}

msg(); // Benny lurks in the shadows <-- after 1 second

// A word of caution however, in the above example each step is done sequentially, with each additional step waiting for the step before to resolve or reject before continuing. If you instead want the steps to happen in parallel, you can simply use Promise.all to wait for all the promises to have fulfilled:

// ...

async function msg() {
  const [a, b, c] = await Promise.all([who(), what(), where()]);

  console.log(`${ a } ${ b } ${ c }`);
}

msg(); // 🤡 lurks in the shadows <-- after 500ms

```

### Promise-Returning

```javascript

// Async functions always return a promise, so the following may not produce the result you’re after:

async function hello() {
  return 'Hello Alligator!';
}

const b = hello();

console.log(b); // [object Promise] { ... }

// Since what’s returned is a promise, you could do something like this instead:

async function hello() {
  return 'Hello Alligator!';
}

const b = hello();

b.then(x => console.log(x)); // Hello Alligator!


// …or just this:

async function hello() {
  return 'Hello Alligator!';
}

hello().then(x => console.log(x)); // Hello Alligator!
```

# Different Forms

```javascript

// Async Function Expression
// Here’s the async function from our first example, but defined as a function expression:

const msg = async function() {
  const msg = await scaryClown();
  console.log('Message:', msg);
}

// Async Arrow Function
// Here’s that same example once again, but this time defined as an arrow function:

const msg = async () => {
  const msg = await scaryClown();
  console.log('Message:', msg);
}
```

### Error Handling

```javascript 

// Something else that’s very nice about async functions is that error handling is also done completely synchronously, using good old try…catch statements. Let’s demonstrate by using a promise that will reject half the time:

function yayOrNay() {
  return new Promise((resolve, reject) => {
    const val = Math.round(Math.random() * 1); // 0 or 1, at random

    val ? resolve('Lucky!!') : reject('Nope 😠');
  });
}

async function msg() {
  try {
    const msg = await yayOrNay();
    console.log(msg);
  } catch(err) {
    console.log(err);
  }
}

msg(); // Lucky!!
msg(); // Lucky!!
msg(); // Lucky!!
msg(); // Nope 😠
msg(); // Lucky!!
msg(); // Nope 😠
msg(); // Nope 😠
msg(); // Nope 😠
msg(); // Nope 😠
msg(); // Lucky!!


// Given that async functions always return a promise, you can also deal with unhandled errors as you would normally using a catch statement:

async function msg() {
  const msg = await yayOrNay();
  console.log(msg);
}

msg().catch(x => console.log(x));

```