---
layout: post
title: React-Update Array->Objext State
date:   2018-12-03 19:07:19
modified:  2018-12-03
comments: true
---

If you have a array object your state like this;

```javascript
constructor(props) {
    super(props);
    this.state = {
        data: [{id: 0, character: 'A'},
               {id: 1, character: 'B'}
        ]
}
```

You can’t update directly;
```javascript
this.setState({ data[0][character]: ‘C’}]
```

If you want to update your state just one object, you can use react-addons-update.

#### Install react-addons-update

```javascript
npm install react-addons-update
```

Import it
```javascript
import update from 'react-addons-update'; // ES6
```

We can use now;

```javascript
this.setState(
  {
    data: update(this.state.data, {
        0: {character: {$set: new1}},
    }),
  }
)
```

[Immutability Helper](http://reactjs.org/docs/update.html)

