# Excercises

### Function parameters depend on each other

* intermediate
* index access type, generalization

```typescript
// HELPERS
class Book {constructor(public title: string){}}
class Song{constructor(public artist: string, public name: string){}}

interface EntityMap {
    book: Book
    song: Song
}

function checkIsBook(kind: string): kind is 'book' {
    return kind === 'book'
}

function checkIsSong(kind: string): kind is 'song' {
    return kind === 'song'
}

function assertUnreachable(x: never): never {
    throw new Error("Didn't expect to get here with x = " + x);
}

// SOLUTION
class Store {
    public songs: Song[] = []
    public books: Book[] = []
    
    public getAll<K extends keyof EntityMap>(kind: K): EntityMap[K][] {
        if(checkIsBook(kind)) return this.books as EntityMap[typeof kind][]
        if(checkIsSong(kind)) return this.songs as EntityMap[typeof kind][]

        return assertUnreachable(kind)
    }
    public add<K extends keyof EntityMap>(kind: K, value: EntityMap[K]): void {}
    public update<K extends keyof EntityMap>(kind: K, id: string, value: Partial<EntityMap[K]>): void{}
}

// TEST
const store = new Store()
const book = new Book('Once upon a time.')
const song = new Song('McHammer', 'Run run')

store.add('song', song)
store.update('song', '123', {name: 'Hammer'})
const storeBooks = store.getAll('book')
```

### Generic type depends on argument of a function

* intermediate
* generalization

```typescript
// IMPLEMENTATION
type Selector<T> = (state: State) => T;
const select = <T>(selector: Selector<T>) => select(selector);

// TEST
type State = {id: string, order: number}
const id = select((state) => state.id) // TYPE string
const order = select((state) => state.order) // TYPE number

```

* intermediate + JS knowledge

```typescript
// IMPLEMENTATION
function unique<T>(array: T[], key: keyof T): T[] {
  return array.filter(
    (e, i) => array.findIndex(a => a[key] === e[key]) === i,
  );
}

// TEST
type Color = {name: string, hex: string}
const colorArray: Color[] = [{name: 'red', hex: '#f00'}, {name: 'red', hex: '#ff0000'}]
const uniqueColorArray = unique(colorArray, 'name') // of type Color[]
```

### Generic different types as function argument&#x20;

* basic/intermediate

```typescript
// IMPLEMENTATION
function onlyWithDefinedNames<T extends {name?: string}>(arg: T) {
  return isDefined(arg.name) ? ({...arg} as T & {name: string}) : null;
}

// TEST
function isDefined<T>(x: T | undefined | null): x is T {
  return !(typeof x === 'undefined' || x === null);
}

type Human = {name: string; height: number};
type Building = {name?: string; creator: Human};

const JaneDoe: Human = {name: 'Jane', height: 150};
const CharlesBridge: Human = {height: 8860};

const objectWithNames = [JaneDoe, CharlesBridge]
  .map(onlyWithDefinedNames)
  .filter(isDefined);

typeof objectWithNames[0].name === 'string';
```

### Generic wrapper for function

* intermediate
* bonus - React solution with React.useCallback
* Task
  * create function \``` getFunctionWrappper` ``, that accept function and return same function type
  * add some inner logic to \``` getFunctionWrappper` ``
  * `bonus - create React.useCallback function`

```typescript
// SOLUTION
import React from 'react';
import {Text} from 'react-native';

type GenericFunc = (...args: any[]) => any;
type SomeFunction = (N1: number, N2: number) => number;

// normal
function getFunctionWrapper<Func extends GenericFunc>(handle: Func): Func {
  return ((...args) => {
    // Add any logic here
    return handle(args);
  }) as Func;
}

// React
function MiddleComponent<Func extends SomeFunction>({handle}: {handle: Func}) {
  const handleWrapper = React.useCallback(
    (...args: Parameters<Func>) => {
      // Add any logic here
      return getFunctionWrapper(handle).apply(null, args);
    },
    [handle],
  ) as Func;

  return <LastComponent handle={handleWrapper} />;
}

function LastComponent<Func extends SomeFunction>({handle}: {handle: Func}) {
  return <Text>{handle(1, 2)}</Text>;
}

// TEST 
const add = (a1:number, a2:number) => a1 + a2
const addWrapper = getFunctionWrapper(add)

```

### Remove first element

* basics/intermediate
* Task
  * create type \`FilterFirstElement`` ` ``, which removes the first element from an array

```typescript
// SOLUTION
type FilterFirstElement<T extends unknown[]> = T extends [unknown, ...infer R] ? R : [];
        
// TEST
type arr1 = FilterFirstElement<[]> // []
type arr2 = FilterFirstElement<[string]> // []
type arr3 = FilterFirstElement<[string, number]> // [number]
type arr4 = FilterFirstElement<[string, number, boolean]> // [number, boolean]
```

### Enhance arguments number

* advance
* Task
  * create type \`Enhance\` to add one more argument (name: string) to any function

```typescript
// SOLUTION
type Enhance<T extends (...args: any) => any> = T extends (
  ...args: infer P
) => infer R
  ? (...args: [...P: P, name: string]) => R
  : never;


// TEST
type foo = (age: number) => void
type fooEnahnced = Enhance<foo>
```

### Type from object key/value

* advance
* Task
  * create type `SpecObject` that will accept object and create data type of only object values for specific keys

```typescript
type Entity = {
  human: {
    name: string;
  };
  animal: {
    age: number;
  };
};

// SOLUTION
type SpecObject<T extends object> = {
  [K in keyof T]: {
    value: K;
    params: T[K];
  };
}[keyof T];

// TEST
type OneEntityType = SpecObject<Entity>;

const person: OneEntityType = {
  value: 'human',
  params: {
    name: 'Tim',
  },
};

const dog: OneEntityType = {
  value: 'animal',
  params: {
    age: 5,
  },
};

```
