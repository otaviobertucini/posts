




[bookmark](https://react.dev/learn/you-might-not-need-an-effect)






## TL;DR

- useEffects are only execute after the re-render of components. So changing a state inside a useEffect will probably cause a new render.
- The trick here is to remove unnecessary `useEffects` and/or move state changes to a single location.
	- The reason for moving state changes to a single location is to make this changes in a sigle pass, so that generating a single render.




- Don’t use states that derives from other states
	- Let’s take Code 1 as an example. `fullName` is deriving from `firstName` and `lastName` . So when one of this states change, `fulName` is changed.
	- The problem with this approach is that when `fullName` changes, first the DOM is re-rendered and then the `useEffect` is called, causing another render.
	- Instead use the approach in Code 2 to avoid errors and to make renders faster.

```javascript
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');

  // 🔴 Avoid: redundant state and unnecessary Effect
  const [fullName, setFullName] = useState('');
  useEffect(() => {
    setFullName(firstName + ' ' + lastName);
  }, [firstName, lastName]);
  // ...
}
```


```javascript
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  // ✅ Good: calculated during rendering
  const fullName = firstName + ' ' + lastName;
  // ...
}
```

- Another common error is to put expensive calculations into states.
- Sometimes it only needs to be stored in variables calculated on renders
- We can also use `useMemo` to avoid recalculating on every render

```javascript
function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');

  // 🔴 Avoid: redundant state and unnecessary Effect
  const [visibleTodos, setVisibleTodos] = useState([]);
  useEffect(() => {
    setVisibleTodos(getFilteredTodos(todos, filter));
  }, [todos, filter]);

  // ...
}

// Instead we can do like

import { useMemo, useState } from 'react';

function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  // ✅ Does not re-run getFilteredTodos() unless todos or filter change
  const visibleTodos = useMemo(() => getFilteredTodos(todos, filter), [todos, filter]);
  // ...
}
```

- Another way to prevent using states is through the use of keys
- When we want to reset the value of some state, we can use an effect to do this
- Instead, we can encapsulate all the states that need to be reset into a single component and the use the `key` prop to reset the states

```javascript
export default function ProfilePage({ userId }) {
  const [comment, setComment] = useState('');

  // 🔴 Avoid: Resetting state on prop change in an Effect
  useEffect(() => {
    setComment('');
  }, [userId]);
  // ...
}

// Instead use the "key" prop

export default function ProfilePage({ userId }) {
  return (
    <Profile
      userId={userId}
      key={userId}
    />
  );
}

function Profile({ userId }) {
  // ✅ This and any other state below will reset on key change automatically
  const [comment, setComment] = useState('');
  // ...
}
```

- 
