# Keywords

## Declaration margin

```typescript
// Type
type Fruit = { name: string }

// Variable
const Fruit = 'jablko'

// Namespace
namespace Fruit {
   const name = 'jablko'
   const color = 'cervena'
   
   export {name, color}
}

// Usages of namespace
Button.Variant
TextInput.Variant
```

## Typeof

obtain a type from a value

* Import

```typescript
async function startConnection(){
    return Promise.all([Promise.resolve(), 'abc', 123]);
}

const defaultConfig = {
    title: 'Basic setup',
    size: '5',
    ...
    theme: 'dark',
}

// Other file
async function handleSetup(){
    const response = await startConnection()

    type ResponseType = typeof response;
    type ConfigType = typeof defaultConfig;
}
```

class

```typescript
class Bank {
    constructor(public readonly coreName: string){}
    static createSuperBank (){return new Bank('superBank')}
}

const bank1 = Bank // Template, model, design, ...
const bank2: Bank = Bank.createSuperBank() // Instance

Bank.c...
bank1.c...
bank2.c...
```

## Keyof

obtain a type from a type

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
}

type DatePropertyNames = keyof Date;
type K1 = keyof Person; // "name" | "age" | "location"

function foo<K extends keyof Person>(entity: Person, key: K){
   return entity[key];
}
```

## Extends - conditional types

### Exclude, Extract

```typescript
type MyExtract<T, U> = T extends U ? T : never
type MyExclude<T, U> = T extends U ? never : T
```

## Infer

Do not use infer too much, slow typescript engine check

* Function

```typescript
// type NameType = {firstName?: string | undefined, lastName?: string| undefined}
function foo(name: {firstName?: string, lastName?: string}){
    return (name.firstName ?? '') + (name.lastName ?? '')
}

// type extends something ✅
// value extends something ❌

type NameType = typeof foo extends (fristParametr: infer A, ...args: any[]) =>  any ? A : never

const mojeJmeno: NameType = {firstName: 'Jan', lastname: 'Hrdy'}

foo(mojeJmeno)
```

* Object

```typescript
const foo = {
    connectName(name: {firstName?: string, secondName?: string}){
        return (name.firstName ?? '') + (name.secondName ?? '')
    }
}

type ConnectNameType = typeof foo extends {'connectName': infer T} ? T : never
type NameType = ConnectNameType extends (fristParametr: infer A, ...args: any[]) =>  any ? A : never

const mojeJmeno: NameType = {firstName: 'Jan', secondName: 'Hrdy'}

foo.connectName(mojeJmeno)

```

## Index access type

```typescript
type ConfigType = {
    scale: string,
    experimental: boolean,
    settings: {
        path: string
        assets: Array<string>
    }
}

type Settings = ConfigType['settings']
type SettingsPath = ConfigType['settings']['path']
```

## Mapped types

* Dict/Record

```typescript
type Fruit = {name: string}
type Dict<T> = {[key: string]: T | undefined} // index signature
type ColorRecord = {[key in 'red' | 'blue']: Color} // mapped types
type MyRecord<K extends keyof any,V> = {[key in K]: V}


// type FruitRecord = MyRecord<string, Fruit>
type FruitRecord= MyRecord<'jabko' | 'pomerance', Fruit>

// const fruits: FruitRecord = {apple: {name:'RED'}, orange: {name: 'Faorie'}}
const fruits: MyRecord<'apple' | 'orange', Fruit> = {apple: {name:'RED'}, orange: {name: 'Faorie'}}

const ovoce: FruitRecord = {
    jabko: {name: 'apple'},
    tresen: {name: 'cherry'},
    pomeranc: {name: 'orange'}
}
```

* Pick

```typescript
const JapaneseFood = {
    name: 'Sushi',
    weight: 150,
    isEatable: true,
}

type MyPick<Value, Keys extends keyof Value> = {
    [key in Keys]: Value[key]
}

type NameFood = MyPick<typeof Food, 'name' | 'weight'>
```

## Mapping modifiers

* read-only

```typescript
type MyReadonly<T> = {
    readonly [P in keyof T]: T[P]
}
```

* optional or not optional

```typescript
// Not optional
type Required<T> = {
    [P in keyof T]-?: T[P]
}

// Optional
type Partial<T> = {
    [P in keyof T]?: T[P]
}
```

## Template literal

```typescript
// Template literals #1
type Mode = 'Dark' | 'Light'
type UpperMode = `${Uppercase<Mode>}-MODE`


// Template literals #2
interface Data {
    digits: Array<number>
    texts: Array<string>
    flags: Record<'darkMode' | 'cached', boolean>
}

type DataSetter = {
    [key in keyof Data as `set${Capitalize<key>}`]: (args: Data[key]) => void
}

function foo(setter: DataSetter){
    setter.setFlags({cached: true, darkMode: false})
}

// Template literals #3
type ExtractADocument<T extends `a${string}` & keyof Document> = T
type def = ExtractADocument<'activeElement' | 'adoptNode'>
```

## Filtering out

```typescript
const data = {
    isReal: false,
    isFake: true,
    digits: [1,2,3],
    id: 'ah6a69h806a8',
    fun: Promise.resolve,
}

type DataType = typeof data
type DefinedValues<T, U> = {[key in keyof T]: T[key] extends U ? key : never}[keyof T]
// type DefinedValues<T, U> = {[key in keyof T]: T[key] extends U ? key : never}[keyof T] & keyof T
type OnlyBooleansDataKeys = DefinedValues<DataType, boolean> 
```
