<div align='center'>
  <h1> Frontend </h1>
  <h2> State Management Patterns and Libraries </h2>
  <h3> Flux </h3>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

---

# Introduction

Flux is an architectural design pattern for managing client-side state and data flow in JavaScript applications. It was introduced by Facebook (Meta) for React-like applications to address issues with two-way data binding (a.k.a. bidirectional updates), such as in [MVVM](../../architectural_patterns/mvvm.md), by replacing them with a strict unidirectional data flow.

As of 2023, [Flux has been archived](https://github.com/facebookarchive/flux) and superseded by [Redux](https://redux.js.org/).

The React community does not prescribe MVVM. Developers typically describe their architecture in terms of unidirectional data flow, component composition, and state management.

However, at scale, a common MVVM-style approach is to use a custom hook per screen or feature as the `ViewModel`, and the global store, API layer, and domain logic as the `Model`. Apply it when screen complexity justifies the extra layer.

---

# References

[1] https://github.com/facebookarchive/flux
