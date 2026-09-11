<div align='center'>
  <h1> Presentation-Layer Architectural Patterns </h1>
  <h2> Model-View-ViewModel (MVVM) </h2>
</div>

# About 

MVVM is a UI architectural design pattern mostly used in frontend development, such as with client-side libraries (e.g., React), to build web/mobile applications at scale, improving maintainability and scalability.

- Model: Represents and manages the application's data and business logic, including state and domain data. It is independent of the UI.

- View: Represents the user interface and presentation layer. It displays data and captures user interactions, delegating state and user actions to the ViewModel.

- ViewModel: Acts as an intermediary between the Model and View. 
  - Exposes State: converts raw data from the Model into properties that the View can easily display. 
  - Exposes Commands: provides actions (e.g., button clicks) that the View can trigger directly via data binding.
  - Handles UI Logic: manages presentation behavior, such as toggling visibility or formatting dates into strings.
  - Mediates Data: fetches data from the Model layer and sends user updates back down to it.