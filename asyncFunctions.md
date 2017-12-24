```javascript
function getUsers (name, age, cb) {...}

function callbackToPromise (method, ...args) {
    return new Promise(function(resolve, reject) {
        return method(...args, function(err, result) {
            return err ? reject(err) : resolve(result);
        });
    });
}

async function getFirstUserName() {
  const user = await callbackToPromise(getUsers, 'userName', 'userAge')
  return user[0].name
}
```
