<div align='center'>
  <h1> 3.2 Frontend </h1>
  <h2> 3.2.5 State Management Patterns and Libraries </h2>
  <h3> Redux </h3>
</div>
  
# Table of Contents

- [Introduction](#introduction)

---

# Introduction

[Redux](https://redux.js.org/tutorials/fundamentals/part-1-overview) is a JavaScript [state](https://reactnative.dev/docs/state) management library that provides a centralized store/single source of truth (Redux store) for managing application state and predictable state updates. It is commonly used to manage shared or global state across an application.

If the app is small or simple, Redux might not be needed, as React's built-in hooks for state management (such as useState and useReducer) may suffice. But for larger applications, Redux can provide significant advantages, such as centralized state management and predictable state updates.

Redux uses reducers to calculate a new state in response to actions. An action is a JavaScript object that describes an event, while a reducer is a pure function that receives the current state and an action as arguments and returns the new state.
