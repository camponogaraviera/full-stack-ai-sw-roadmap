<div align='center'>
  <h1> Presentation-Layer Architectural Patterns </h1>
  <h2> Model-View-Presenter (MVP) </h2>
</div>

# About 

MVP is a UI architectural design pattern mostly used in frontend development.

- Model: Manages the application's data, business logic, and state. It is responsible for retrieving, storing, and manipulating data, independently of the UI.

- View: User interface and presentation layer that displays data from the Model and captures user interactions.

- Presenter: Acts as the intermediary between the Model and View. It handles user interactions received from the View, applies presentation and application logic, retrieves or updates data through the Model, and instructs the View on what to display.