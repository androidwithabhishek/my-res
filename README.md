



The raw URL for your file is:

https://raw.githubusercontent.com/androidwithabhishek/my-res/main/Screenshots/AuthScreen.jpeg
This works because you replace:

github.com → raw.githubusercontent.com

Remove /blob

So:

https://github.com/androidwithabhishek/my-res/blob/main/Screenshots/AuthScreen.jpeg
becomes:

https://raw.githubusercontent.com/androidwithabhishek/my-res/main/Screenshots/AuthScreen.jpeg
If this URL gives a 404, then check:

The repository is public.

The file name is exactly AuthScreen.jpeg (GitHub is case-sensitive).

The file has been pushed to the main branch. 


DashboardScreen.jpeg

The raw URL for your image is:

https://raw.githubusercontent.com/androidwithabhishek/my-res/main/Screenshots/DashboardScreen.jpeg
If it doesn't open, check that:

The repository is public.

The file name is exactly DashboardScreen.jpeg (GitHub is case-sensitive).

The file has been pushed to the main branch. 


i made navigation app so use both url and write a nice md with modern way with professionalism and explain it  

Here's a modern, professional README.md for your navigation app. Replace <YOUR_REPO_LINK> with your repository URL.

# 🧭 Navigation App

A modern Android application demonstrating a **scalable, modular, and type-safe navigation architecture** built with **Jetpack Compose**. This project showcases how to organize navigation professionally in large Android applications using feature modules and Clean Architecture principles.

---

## 📱 Screenshots

<div align="center">

| Authentication | Dashboard |
|:--------------:|:---------:|
| <img src="https://raw.githubusercontent.com/androidwithabhishek/my-res/main/Screenshots/AuthScreen.jpeg" width="260" alt="Authentication Screen"/> | <img src="https://raw.githubusercontent.com/androidwithabhishek/my-res/main/Screenshots/DashboardScreen.jpeg" width="260" alt="Dashboard Screen"/> |

</div>

---

## ✨ Features

- 🔐 Authentication flow
- 🏠 Dashboard navigation
- 🧩 Multi-module architecture
- 🛣️ Type-safe navigation
- 📦 Feature-based navigation graphs
- ⚡ Jetpack Compose UI
- 🧼 Clean Architecture
- ♻️ Reusable navigation APIs
- 🎯 Scalable project structure

---

## 🏗️ Architecture

The project follows a modular architecture where each feature owns its own navigation graph and UI.

```
app
│
├── core
│   ├── navigation
│   ├── common
│   └── ui
│
├── feature-auth
│
├── feature-dashboard
│
└── feature-profile
```

This structure allows every feature to remain independent, reusable, and easier to maintain as the application grows.

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



