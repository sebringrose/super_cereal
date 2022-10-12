# cerealizer
Super cereal serialization without any weirdness. Retains methods and circular references 🥣

## But how?

```
import { Store, Model } from "./mod.ts";

const store = new Store();

class Hobby extends Model {
  #title: string;
  
  constructor(title: string) {
    super(store, arguments);
    this.#title = title;
  }

  getTitle () { return this.#title };
}

const fencing = new Hobby("fencing");
const storedId = fencing.save();

const deserializedFencing = store.load(storedId) as Hobby;
```

The Al-Gore-itms use some clever recursive logic to leave unique IDs on objects as they depth first search the data structure. This allows for serialization of nested objects without getting stuck in circular reference loops.

Simply instantiate a `Store` then make your classes extend `Model` and call `super(store, arguments)` in your constuctors. This allows the store to reinstantiate your classes before  using `Object.assign` to apply deserialized object values. 

You get to keep your lovely graph structure and all the lovely methods on your objects too.

## But why?

I don't want to use complex databases. I just want to serialize and store, whether in the browser on the server. The goal was to require minimal additional code beyond regular looking JS/TS and I think this fits the bill nicely. The only slight caveat is that you have to use `store.load(id) as ClassName;` to keep accurate TS syntax highlighting (there is currently no way for TS to infer this an generic types don't work with recursion).
