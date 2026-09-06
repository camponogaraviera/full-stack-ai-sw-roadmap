<div align='center'>
  <h1> 3.1.2 Presentation-Layer Architectural Patterns </h1>
</div>

# Table of Contents

- [Model-View-Controller (MVC)](#model-view-controller-mvc)

---

# Model-View-Controller (MVC)

MVC is a UI architectural design pattern used in both backend and frontend development. However, it is commonly used by server-side web application frameworks such as [Laravel](https://github.com/laravel/framework), [Spring Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html), [ASP.NET Core MVC](https://learn.microsoft.com/zh-tw/aspnet/core/mvc/overview?view=aspnetcore-10.0), and [Ruby on Rails](https://rubyonrails.org/).

- Model: Represents application data and encapsulates domain/business logic. It provides data to the Controller.

- View: Renders the user interface.

- Controller: Handles user input and application requests, coordinates application logic, and interacts with the Model before selecting or updating the View.
