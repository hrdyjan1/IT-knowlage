---
description: Information from JS/TS language, that are tricky, but good to know.
---

# Challenging knowledge

### Difference between return/return await

Function with await will be cathed in `catch block`

```javascript
const fakeFetch = () => new Promise((_, rej) => setTimeout(() => rej('error'), 500));

async function asyncFunctionWithAwait() {
  try {
    return await fakeFetch().then(() => 'V pohode');
  } catch (e) {
    console.log('Chyba');
  }
}

async function asyncFunctionWithoutAwait() {
  try {
    return await fakeFetch().then(() => 'V pohode');
  } catch (e) {
    console.log('Chyba');
  }
}
```

### Deterministic
