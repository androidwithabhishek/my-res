
---

## 🚀 Navigation Flow

```
App
 │
 ▼
Authentication
 │
 ▼
Dashboard
 │
 ├── Profile
 ├── Settings
 └── More Features
```

Each feature registers its own navigation graph instead of placing every destination inside a single `NavHost`.

---

## 💡 Why This Architecture?

Instead of keeping all screens inside one navigation graph, every feature exposes its own navigation API.

Benefits include:

- Better scalability
- Easier maintenance
- Feature isolation
- Cleaner dependencies
- Improved readability
- Faster development for large teams

This approach is commonly used in production Android applications with multiple modules.

---

## 🛠️ Tech Stack

- Kotlin
- Jetpack Compose
- Navigation Compose
- Kotlin Serialization
- MVVM
- Clean Architecture
- Multi Module
- Material 3
- Coroutines
- StateFlow

---

## 📂 Project Structure

```
📦 app
📦 core
 ├── navigation
 ├── common
 └── ui

📦 feature-auth
 ├── api
 ├── impl
 └── presentation

📦 feature-dashboard
 ├── api
 ├── impl
 └── presentation
```

---

## 🎯 Learning Goals

This project demonstrates:

- Professional navigation architecture
- Multi-module application development
- Feature-based navigation
- Type-safe routes
- Separation of concerns
- Clean project organization

---

## 🚀 Getting Started

1. Clone the repository

```bash
git clone <YOUR_REPO_LINK>
```

2. Open the project in Android Studio.

3. Sync Gradle.

4. Run the application on an emulator or physical device.

---

## 📸 Preview

Authentication Screen

<img src="https://raw.githubusercontent.com/androidwithabhishek/my-res/main/Screenshots/AuthScreen.jpeg" width="300"/>

Dashboard Screen

<img src="https://raw.githubusercontent.com/androidwithabhishek/my-res/main/Screenshots/DashboardScreen.jpeg" width="300"/>

---

## 🤝 Contributions

Contributions, suggestions, and improvements are always welcome. Feel free to fork the repository and open a pull request.

---

## ⭐ Support

If you found this project helpful, consider giving it a ⭐ on GitHub. It helps others discover the project and motivates further improvements.
This style is similar to the polished READMEs you see on popular Android GitHub projects: it has a clean header, side-by-side screenshots, a clear architecture explanation, project structure, tech stack, and a professional layout without being overly verbose.



