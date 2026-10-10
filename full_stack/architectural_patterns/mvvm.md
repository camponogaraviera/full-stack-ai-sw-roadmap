<div align='center'>
  <h1> Presentation-Layer Architectural Patterns </h1>
  <h2> Model-View-ViewModel (MVVM) </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [Components](#components)
- [Mapping MVVM to React with Hooks](#mapping-mvvm-to-react-with-hooks)
- [Q&A](#qa)
- [References](#references)

# Introduction

Model-View-ViewModel (MVVM) is a UI architectural design pattern used to separate the user interface (the `View`) from the domain model (the `Model`). It typically uses a two-way data binding (a.k.a. bidirectional update) to keep the `View` and `ViewModel` in sync.

It was proposed by Ken Cooper and Ted Peters to simplify event-driven programming of user interfaces.

It is commonly used in UI-heavy applications with complex screen-level logic, improving maintainability and scalability.

---

# Components

- `Model`: Represents the application's domain model that includes domain data/state and business rules/logic. It is independent of and isolated from the `View`. This component reaches the backend only through an API client. Direct DB access belongs to the backend.

- `View`: Represents the user interface and presentation layer. It renders/displays model content and delegates user interactions (button clicks, keyboard inputs) to the `ViewModel` via data binding (e.g., properties and callbacks).

- `ViewModel`: Its job is to prepare and format data from the `Model` for the `View` to consume, and to handle user interactions passed down from the `View`.
  - Exposes State: Transforms raw data/state from the `Model` into properties that the `View` can display.
  - Exposes Commands: Translates UI gestures into application commands. Provides actions that the `View` can invoke through data binding.
  - Handles Presentation (UX) Logic: Manages presentation-related behavior, such as toggling visibility state, enabling/disabling controls, performing client-side data validation (e.g., "email field cannot be empty" or "password must contain at least 8 characters"), and formatting data for display.
    - Note that client-side data validation is for UX only. The backend must re-validate, which is the domain's job in a Hexagonal Architecture.
  - Coordinates Data: Retrieves data from and sends user-driven updates to the `Model` or underlying application/domain services.

- `Binder`: Is the data-binding mechanism that keeps the `View` and `ViewModel` in sync.
  - In WPF, the binding engine resolves binding expressions declared in XAML against the `DataContext` and updates the UI whenever the `ViewModel` raises `PropertyChanged`.
  - In React, there is no dedicated data binder. Instead, its declarative rendering, where state or props change and the component re-renders, acts as the one-way data binder.

---

# Mapping MVVM to React with Hooks

The React community does not prescribe MVVM. Developers typically describe their architecture in terms of component composition, unidirectional data flow, and state management. 

However, at scale, a common MVVM-based style mapping is to use custom hooks to encapsulate the `ViewModel's` presentation logic, while client-side global state management, data fetching operations, and domain logic constitute the `Model`. Apply MVVM when the complexity justifies.

- `View`: Includes functional JSX components that render UI from props or from a `ViewModel` hook's return values. They contain no domain logic, only presentation concerns.
  - Examples of components: `SignUpScreen.tsx`.

- `ViewModel`: Presentation logic layer, including custom hooks that select global state from the `Model` store (e.g., Redux), own local UI state (e.g., useState), format and convert data from `Model` for presentation, and expose actions/commands (e.g., search, form submission, UX validation).
  - Examples of custom Hooks: `useSignUpViewModel.ts`, `useUserProfileViewModel.ts`, `useCartViewModel.ts`, `useOrdersViewModel.ts`.

- `Model`: Includes client-side global state management (e.g., Redux or Zustand), server-state operations (e.g., TanStack Query and API clients), and domain logic (e.g., entities, value objects, business rules, and domain services).
  - Server-state operations:
    - API client: `authApi.ts` (module that communicates with the authentication API, e.g., a `signUp()` function).
  - Examples of domain logic:
    - Entities: `User.ts` and `Order.ts`.
    - Value objects: `Money.ts` and `Email.ts`.
    - Business rules: An order cannot be cancelled after it has been shipped.
    - Domain services: `orderService.ts` (pure functions implementing domain logic, e.g., `calculateCartTotal()`).

---

# Q&A

Suppose the frontend was developed using the MVVM-based style approach and the backend using the [Hexagonal Architecture](./hexagonal.md). The user clicks on the sign-up button and submits a form. Which MVVM component handles the communication with the backend? Will the backend trigger a primary or a secondary adapter?

Answer:

In a clean MVVM-based frontend paired with a Hexagonal backend, the frontend communicates with the backend through an API/data-access layer handled by the `Model` layer, typically invoked by a `ViewModel` command.

The request enters the backend through a primary (driving) adapter, and the use case then calls secondary (driven) adapters.

Workflow:

1. **View**: User clicks the "Sign Up" button.

2. **ViewModel**: Receives the command from the `View`, performs presentation/form validation, and calls the `Model`.

3. **Model**: Uses its API/data-access (e.g., fetch(`${API_URL}/users`) layer to send the HTTP request to the backend, then returns the result or error status to the `ViewModel`.

4. **Backend's Primary (Driving) Adapter**: The HTTP controller is the primary adapter that receives the HTTP request and invokes the appropriate primary port/use case (e.g., `UserUseCases`) implemented by `UserService` in the application core.

5. **Backend's Secondary (Driven) Adapter**: The use case calls a secondary port (e.g., `UserRepository`), implemented by a secondary adapter (e.g., `DynamoDBUserRepository`) that saves the user to the database.

Refer to [Hexagonal Architecture](./hexagonal.md) for an example of the backend.

---

# References

[1] https://en.wikipedia.org/wiki/Model–view–viewmodel

[2] https://learn.microsoft.com/en-us/dotnet/architecture/maui/mvvm
