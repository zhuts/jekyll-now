---
layout: post
title: "Writing and Testing Redux Actions with Mocha and Expect"
categories: Redux Testing JavaScript
---

Actions are the triggers that called within your app whenever you want to change the state. They are functions that, when called, broadcasts the action name along with whatever data you wish to send with the action.

Let's say you have a userlist that a part of your state, and you want to add a user to your userlist (and thereby changing the current state). The action function may look like this:

```javascript
export const addUser = (newUser) => {
	return {
		type: 'ADD_USER',
		newUser
	};
};
```

The test that we want is one that checks that, when the addUser funciton is called, 'ADD_USER' is broadcasted to the reducers, and carries with it the passed in newUser. We'll use mocha along with the expect.js assertion library to write our test. Don't forget to import expect.js and your actions!

```javascript
import expect from 'expect'
import * as actions from '../whereever/yous/actions/aresaved'

describe ('My apps cool apps actions', () =>{
	it ('addUser should create an ADD_USER action that carries with it the passed in new user', () => {
		expect(actions.addUser('Joe')).toEqual({
			type: 'ADD_USER',
			newUser: 'Joe'
		});
	});
});

```

Note that this method of writing action tests should work regardless of what framework you're working in. React, React-Native, Angular... it shouldn't matter, as long as you're working in Redux.