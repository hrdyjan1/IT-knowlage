---
description: General knowledge about functional programming.
---

# Functional programming

## What is a proper/pure function?

The function which is TOTAL, DETERMINISTIC, NO SIDE EFFECTS

### Total

Answer for all questings ( all domains have the range)

```javascript
// Missing results for other numbers than 1
const inc = i => {
    if(i === 1) return 2
}
```

### Deterministic

Same output all the time

### No side effects

Do only necessary things to do

Do not mutate data

## Facts

* Reliable (same input => same output)
* Not specific to one place only (take it and move to other codebases)
* Reusable (run the same function over and over)
* Testable (guarantee that it works)
* Composable (small building blocks)



* Short names (x,y,z,...)
* Data operates last

```javascript
// xs represents data, is the last param-
const filter = f1 => f2 => xs => xs.filter(f1).filter(f2)
```
