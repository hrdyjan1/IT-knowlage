# TOP NOTCH

### Word kind

In TS we do not use word type as a value. Type is the keyword. Instead, we use the word **kind.**

### **Interface vs Type**

Multiple declarations of the same interface name.

```typescript
interface Entity {
    name: string
}

interface Entity {
    digits: number
}

function foo(e: Entity){
    e.name
    e.digits
}
```
