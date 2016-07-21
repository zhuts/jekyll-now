---
layout: post
title: "Writing and Testing Redux Reducers with Mocha and Expect"
categories: Redux Testing JavaScript
---

Reducers are used to change state in a Redux application. Each reducer works on a different part of the state, listening for specified actions to be called.

In the example below, our state has a userlist and is listening for "ADD_USER" to be broadcast. If it sees "ADD_USER", it will grab what ever data is attached to the "ADD_USER" action and recalculate the state.

For reference, here is our ADD_USER action:
```javascript
export const addUser = (newUser) => {
	return {
		type: 'ADD_USER',
		newUser
	};
};
```
Note that we have to be careful about side effects with Redux. More specifically, the new piece of state that is returned by the reducer can't just be the current state transformed; it needs to be a seperate, new object.

In our state, our userlist is an array. To add a new user to an array instinctually you'd just use the .push method, like so:

```javascript
export default (state = [], action) => {
	switch (action.type) {
		case 'ADD_USER':
			state.push(action.newUser);
			return state;
	}
};

```
While this is still functional, remember the .push method actually transforms the array that it works on... the state that is being returned is actually the previous state, which can have unintended consequences when working with complicated applications.

The test that we will use will reflect the constraint by using a freeze on the state passed in. For these tests we'll use expect.js for assertions and deep-freeze-strict to freeze the prior state objects.

Note the basic form of the test. First the starting state is defined, along with the action and the desired resulting state after both are passed through the reducer. Then prior state is frozen to prevent any outside affects on the state. Then you take the result of passing the prior state and the action to the reducer and compare it to the desired result.

```javascript
import expect from 'expect';
import deepFreeze from 'deep-freeze-strict';
import userReducer from '../whereever/userReducer'

describe ('userReducer', ()=>{
  it('should add a new user to the user list when ADD_USER is called', () => {

    const stateBefore = [
    	'Nick',
    	'Jenny'
    ];

    const action = {
      type: 'ADD_USER',
      newUser: 'Joe'
    };

    const stateAfter = [
      'Nick',
      'Jenny',
      'Joe'
    ];

    deepFreeze(stateBefore);
    expect(userReducer(stateBefore, action)).toEqual(stateAfter);
  });
})

```

If you setup the userReducer using the .push method, this test will fail because the deep freeze will prevent the .push from operating on the prior state because .push is transformative. 

Instead we need to create a new copy of the state with the added user, and we can accomplish that using the Object.create method to create an exact copy of prior state, and then use .concat to add on the new User.

```javascript
export default (state = [], action) => {
	switch (action.type) {
		case 'ADD_USER':
			return Object.create(state).concat(action.newUser);
	}
};
```
To further clean up your code, you can also use the es6 spread operator, which can be used to create a new array that includes an existing array.

```javascript
export default (state = [], action) => {
	switch (action.type) {
		case 'ADD_USER':
			return [...state, action.newUser];
	}
};
```
