---
layout: post
title: React-Use Css Modules in create-react-app
date:   2018-11-30 19:07:19
modified:  2018-11-30
comments: true
---

We can easily use css module in create-react-app.

#### First Step — Create app
```javascript
    create-react-app app_name
```

#### Secon Step — Change file extension
Change your css file name. For instance if your css file name is styles.css, new file extension is styles.module.css. You don’t need to install anything else to make css modules work.

#### Third Step-Use inside the component
We can use this css module now.

styles.module.css
```
    .avatar {
        width: 250px;
        height: 250px;
        border-radius: 8px;
    }
```
We don’t need to export our css module file. But we have to import inside our component.

UserInfo.js
```javascript
import styles from './styles.module.css'
import React, { Component } from 'react';
const UserInfo = ({user}) =>{
    return(
        <div>
            { user &&
              <div>
                 <img className={styles['avatar']}  src={user['avatar_url']} />
              </div>
            }
       </div>
}
export default UserInfo
```
Resources:

[Css modules](http://www.robinwieruch.de/create-react-app-css-modules/)
