---
layout: post
title: Redux Firebase-How to retrieve data from one single userID?
date:   2018-12-03 19:07:19
modified:  2018-12-03
comments: true
img: redux-firebase3.png
---

We can retrieve data from on single userId in firebase.

I have a pomodoros collection on mm firebase cloud database. And I am saving these records.

![Redux Firebase]({{ site.url }}/assets/img/redux-firebase.jpg)

I’m saving the pomodoros of user. Every user has got different uid.

I have create an action for save pomodoro record.

```javascript
export const createPomodoro = (pomodoro) => {
    return (dispatch, getState, { getFirebase, getFirestore }) => {
        const firestore = getFirestore();
        const profile = getState().firebase['profile'];
        const userId = getState().firebase['auth']['uid'];
        firestore.collection('pomodoros').add({
             session_type: pomodoro.session,
             description: '',
             userId: userId,
             createdAt: new Date(),
             weekNumOfYear: new Date().getWeekNumber(),
             dayNumOfWeek: new Date().getDayOfWeek()
    }).then( () => {
         dispatch({ type: 'CREATE_POMODORO', pomodoro })
    }).catch((err) => {
         dispatch({ type: 'CREATE_POMODORO_ERROR', err });
    })
  }
}
```

Also I have got pomodoro reducer for control state.

```javascript
const pomodoroReducer = (state = initState, action) => {
    switch (action.type){
        case 'CREATE_POMODORO':
            return state;
        case 'CREATE_POMODORO_ERROR':
            return state;
        default:
            return state;
   }
    return state;
}
```
I want to get the pomodoros that each user creates. After that, i want to display these records. I coded an component to these actions.

```javascript
const mapStateToProps = (state) => {
     return {
         auth: state.firebase.auth,
         pomodoros: state.firestore.ordered.pomodoros
     }
}
export default compose(connect(mapStateToProps),
    firestoreConnect((props) => {
        if (!props.auth.uid) return []
        return [
            {collection: 'pomodoros',
            where: [
               ['userId', '==', props.auth.uid]
            ]
        }
        ]
    }))(Statistic);
```

There is an most important part. I’m connecting Firestore with firestoreConnect function. And asking; send me all pomodoros my authenticate userId please. And everything will be working correctly.

We can use these data now. 👯

![Redux Firebase]({{ site.url }}/assets/img/redux-firebase2.jpg)

You can see all source code [here.](http://github.com/ebrugulec/pomodoro-timer)