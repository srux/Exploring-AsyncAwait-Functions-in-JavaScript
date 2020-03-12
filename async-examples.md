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
```


