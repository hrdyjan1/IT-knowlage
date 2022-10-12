# Interface vs Type

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
