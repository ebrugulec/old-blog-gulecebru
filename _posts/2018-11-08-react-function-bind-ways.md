---
layout: post
title: React-Function Bind Ways
date:   2018-11-08 19:07:19
modified:  2018-11-08
comments: true
img: react-bind-ways.png
---

We can bind to function a few ways in react. Some ways are useless or they can create performance problem. But you can choose better way for you.

####Dynamic binding in render()

This approach is terse and clear, however, there are performance implications since the function is reallocated on every render.

```javascript
class HelloWorld extends Component {
  handleClick(event) {}
  render() {
    return (
      <p>Hello, {this.state.name}!</p>
      <button onClick={this.handleClick.bind(this)}>Click</button>
    );
  }
}
```
####Use Arrow Function in Render
This approach has the same potential performance impact render bind

```javascript
onChange={e => this.handleChange(e)}
```

####Bind in constructor()

```javascript
If we are using Component class in React, We can bind our function inside the constructor.
```

```javascript
class HelloWorld extends Component {
  constructor() {
    this.handleClick = this.handleClickFunc.bind(this);
  }
  render() {
    return (<button onClick={this.handleClick}/>);
  }
}
```

####Bind with the Arrow Function

```javascript
class HelloWorld extends Component {
  handleClick = (event) => {
    console.log(this.state.name);
  }
  render() {
    return (<button onClick={this.handleClick}/>)
  }
}
```

####Dynamic binding with the Arrow Function for multiple elements

```javascript
class HelloWorld extends Component {
  handleChange = name => event => {
    this.setState({ [name]: event.target.value });
  }
  render() {
    return (
      <input onChange={this.handleChange('name')}/>
      <input onChange={this.handleChange('description')}/>
    )
  }
}
```

####React.createClass
If you use React.createClass, React autobinds all functions to this. However, with the advent of ES6 classes, this non-standard approach to creating classes isn’t the future of React.

Resources:

[React Bind Function](http://medium.freecodecamp.org/react-binding-patterns-5-approaches-for-handling-this-92c651b5af56)





