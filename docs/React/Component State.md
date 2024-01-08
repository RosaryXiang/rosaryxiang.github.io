---
layout: default
title: Managing Component State
parent: React
nav_order: 1
---

# State Hook

1. React updates states aynchronously

   Rerender after {} has ended.

2. State is stored outsides of components

   Outside of App()

3. Use hooks at the top level of your component

   Cannot define in for loop

# Choosing the State Structure

1. Avoid Redundant state variables

   If a variable can be calculated by others

2. Group related variables inside an object

   ```tsx
   const [name, setName] = UseState({
     firstName: "Jessie",
     lastName: "Li",
   });
   ```

3. Avoid deeply nested structures

# Keeping Component Pure

Inpure Component:

```tsx
let count = 0
function Message = () => {
	count++;
	return <div> Message {count} </div>;
}
export default Message;
```

# Understanding Strict Mode

```tsx
ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

Strict mode aims to detect potential issues of the codes. It will redender components twice.

The first time is to detect issues. The ouput of the second time is used to update user interface.

# Updating Object

```tsx
const [drink, setDrink] = useState({
  title: "Orange Juice",
  price: 5,
});

const handleClick = () => {
  setDrink({ ...drink, price = 6 });
};
```

# Updating Nested Object

```tsx
const [customer, setCustomer] = useState({
  name: "Jessie",
  address: {
    city: "New York",
    zipCode: 92611,
  },
});

const handleClick = () => {
  setCustomer({
    ...customer,
    address: { ...customer.address, zipCode: 92511 },
  });
};
```

Because, in this case, we don’t want shalow copy, we need to construct a new address object.

# Updating Arrays

```tsx
const [tags, setTags] = useState(["happy", "cheerful"]);

const handleClick = () => {
  //Add
  setTags([...tags, "exciting"]);
  //Remove
  setTags(tags.filter((tag) => tag !== "happy"));
  //Update
  setTags(tags.map((tag) => (tag === "happy" ? "happiness" : tag)));
};
```

# Updating Objects

```tsx
const [bugs, setBugs] = useState([
  { id: 1, title: "bug 1", fixed = false },
  { id: 2, title: "bug 2", fixed = false },
]);

const handleClick = () => {
  setBugs(bugs.map((bug) => (bug.id === 1 ? { ...bug, fixed = true } : bug)));
};
```

B1, B2 ⇒ B1\*, B2

we don’t create new elements for a brand new array, we only create new element for the element being modified.

# Simplying Update Logic with Immer

```tsx
import produce from "immer";

const [bugs, setBugs] = useState([
  { id: 1, title: "bug 1", fixed = false },
  { id: 2, title: "bug 2", fixed = false },
]);

const handleClick = () => {
  //setBugs(bugs.map(bug => bug.id === 1 ? {...bug, fixed = true} : bug));
  setBugs(
    produce((draft) => {
      const bug = draft.find((bug) => bug.id === 1);
      if (bug) bug.fixed = true;
      í;
    })
  );
};
```

`produce` takes a base state, and a *recipe* that can be used to perform all the desired mutations on the `draft` that is passed in. The interesting thing about Immer is that the `baseState` will be untouched, but the `nextState` will reflect all changes made to `draftState`.

Inside the recipe, all standard JavaScript APIs can be used on the `draft` object.

# Sharing States between Components

If two components share a set of states, these states should be declear in their nearest parent component.

# Exercise Updating State

### First

```tsx
function App() {
  const [game, setGame] = useState({
    id: 1,
    player: {
      name: "John", // change it to "Bob"
    },
  });
  const handleClick = () => {
    setGame({ ...game, player: { ...play, name: "Bob" } });
    //it should be
    //setGame({...game, player : {...game.play, name : "Bob"}})
  };
}
```

### Second

```tsx
functin App(){
	const [pizza, setPizza] = useState({
		name : "spicy pizza",
		toppings : ["Mushroom"], //Add "Cheese"
	});
	const setHandler = ()=>{
		setPizza({...pizza, toppings : [...pizza.toppings, "Cheese"]})
	}
}
```

### Third

```tsx
function App() {
  const [cart, setCart] = useState({
    discount: 0.1,
    items: [
      { id: 1, title: "Product 1", quantity: 1 }, //set quantity to 2
      { id: 2, title: "Product 2", quantity: 1 },
    ],
  });
  setCart(
    produce((draft) => {
      const item = cart.items.find((item) => item.id === 1);
      if (item) item.quantity = 2;
    })
  );
}
```

# Exercise Building an expandableText Component

```tsx
import { useState } from "react";
import Button from "./Button";

interface Props {
  children: string;
  maxNum?: number;
}

const ExpandableText = ({ children, maxNum = 100 }: Props) => {
  const [textExtend, setTextExtend] = useState(false);
  return (
    <>
      <div>
        {textExtend ? children : children.substring(0, maxNum)}
        <Button onClick={() => setTextExtend(!textExtend)}>
          {textExtend ? "Less" : "More"}
        </Button>
      </div>
    </>
  );
};

export default ExpandableText;
```
