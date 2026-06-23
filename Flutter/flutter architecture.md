# Advanced Flutter Architecture Curriculum

هذه هي الخطة الشاملة لتعلم معمارية فلاتر المتقدمة (Advanced Flutter Architecture).

## Lectures Overview

- **Lecture 1**: The Three Trees: How Flutter Renders UI Under the Hood
- **Lecture 2**: State Management Foundations: setState vs InheritedWidget
- **Lecture 3**: Dependency Injection: Decoupling Code with GetIt
- **Lecture 4**: Event-Driven State: Mastering the BLoC Pattern
- **Lecture 5**: Clean Architecture: The Domain Layer and Use Cases
- **Lecture 6**: The Data Layer: Repositories and Error Handling
- **Lecture 7**: Network & Local Storage: Dio Interceptors and Hive
- **Lecture 8**: Routing Architecture: Declarative Navigation with GoRouter
- **Lecture 9**: Modular Architecture: Feature-First Folder Structure
- **Lecture 10**: Testing the Architecture: Unit, Widget, Integration, and Mocking
- **Lecture 11**: Final Mini-Project: Offline-First App with Clean Architecture

---

## Detailed Notes

### Lecture 1

- **Widget Tree vs Element Tree vs RenderObject Tree**
    - **The Widget is destroyed and recreated** from scratch every time a change occurs, because it is extremely lightweight and consumes very little memory.
    - **The Element stays in its place**; it acts as the brain that links the new widget to the exact same spot on the screen.
    - **The RenderObject is not destroyed**; instead, it receives a command from the Element to only update the property (like changing the color), preserving the app's speed and performance.
    
    > **Summary**
    > - Declarative UI means describing the current state, not giving step-by-step drawing commands.
    > - The Widget Tree is just a cheap, temporary configuration (the menu).
    > - The Element Tree manages the lifecycle and links widgets to what is actually rendered.
    > - The RenderObject Tree does the heavy lifting of painting pixels on the screen.
    > 
    > **Key rule to remember:** Widgets are immutable (unchangeable) and rebuilt constantly, while Elements and RenderObjects are kept alive as long as possible for performance.
    > 
    > **Common mistake to avoid:** Thinking that rebuilding a Widget means the actual screen pixel (RenderObject) is completely destroyed and recreated from scratch.
    
- **Framework vs Engine & StatefulWidget Lifecycle**
    
    > **Summary**
    > - The Framework (Dart) is what we write; the Engine (C++) is what actually draws and talks to the OS.
    > - `initState` is for setup and runs exactly once.
    > - `build` runs repeatedly whenever the state changes.
    > - `dispose` is critical for cleaning up memory (like closing streams or controllers) before the widget dies.
    > 
    > **Key rule to remember:** Never leave a controller, stream, or timer running without stopping it inside `dispose`.
    > 
    > **Common mistake to avoid:** Trying to access `context` to show a dialog or navigate inside `initState` without caution, because the widget is not fully attached to the screen yet.

### Lecture 2

- **Ephemeral vs App State & `ValueNotifier`**
  Prop-drilling & InheritedWidget - **Prerequisites:** StatefulWidget Lifecycle.
    
    > **Summary**
    > - Ephemeral state is local and managed by `setState` or `ValueNotifier` inside a single widget.
    > - App state is global and needed by multiple widgets across the app.
    > - Prop Drilling is the bad practice of passing data down through constructors of widgets that don't actually need it.
    > - InheritedWidget allows widgets down the tree to instantly access data from up the tree without manual passing.
    > 
    > **Key rule to remember:** If a variable is only used inside one widget (like a loading spinner toggle), use `setState` or `ValueNotifier`. If it's needed by multiple screens (like a user auth token), treat it as an App State.
    > 
    > **Common mistake to avoid:** Overusing App State (like Provider/BLoC) for simple, local animations or toggles, which wastes memory. Keep local things local with `setState` or `ValueNotifier`.

### Lecture 3

- **Service Locator (GetIt) & Tight Coupling vs Loose Coupling**
    
    > **Summary**
    > - Tight Coupling makes classes create their own dependencies, making testing impossible.
    > - Loose Coupling passes dependencies from the outside.
    > - GetIt acts as a global Service Locator, allowing any screen to access registered services instantly.
    > - `registerLazySingleton` creates exactly one instance of a service, and only when it is requested for the first time, saving memory.
    > 
    > **Key rule to remember:** UI Widgets should NEVER instantiate logic classes or network services using the `()` keyword directly inside their build method.
    > 
    > **Common mistake to avoid:** Registering everything as a `Singleton` even if it's only needed temporarily, which fills up the device's memory unnecessarily. Use `registerFactory` if you want a fresh instance every time.
    
- **Provider as DI & GetIt**
    
    > **Summary**
    > - Provider is not just for state management; it is a powerful Dependency Injection tool.
    > - GetIt provides services globally (no `context` needed), which is great for raw business logic.
    > - Provider provides services hierarchically (needs `context`), which keeps the UI cleanly scoped and isolated.
    > 
    > **Key rule to remember:** When using Provider just to pass a service class (like `ApiService`), use the base `Provider<T>` widget and set `listen: false` when reading it, because services don't trigger UI updates.
    > 
    > **Common mistake to avoid:** Trying to read a Provider outside of the widget tree (e.g., inside a background Isolate or before the UI is fully built), which will crash the app because there is no `context` available.
