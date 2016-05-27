---
layout: post
title: 2016-05-27 Classes and Prototypes in Javascript
---

Sometimes there is a need for multiple objects that share common
traits/methods, but also have their own unique traits/methods. There are
multiple methods available to you in JS for just such a case.

##PsuedoClassical

Similar to Java's class system. Uses a declared _constructor function_, denoted
by the capitalization of the function name. New objects of the constructor types
are instantiated through the use of the _new_ keyword. These new objects will
have all of the methods currently declared on the constructor function.
Additional methods can be added to the constructor by using
constructorName._prototype_.(methodName), but note that already instantiated
objects will not have the new method placed on them. Instead if the new method
is called on the previously existing object, and assuming the method doesn't
already exist on the object, the object will look to its constructor and, if the
method exists there, will execute the method as if it existed on the object.

```javascript
function Dog(name) {
  this.name = name;
}

var myDog = new Dog("Hambone");

console.log(myDog.name); //"Hambone"

console.log(Object.keys(myDog)) //["name"]

Dog.prototype.speak=function(){
  console.log("RUFF");
}

myDog.speak(); //"RUFF"

console.log(Object.keys(myDog)); //["name"]

```

This creates an extremely economical way to "add" methods and properties to
similar, pre-existing objects, without having to actually add the new method to
each individual object. The new method only has to exist on the parent
constructor.
