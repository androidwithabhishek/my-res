# 🧭 Navigation in a Multi-Module Android App

A minimal, practical example showing how to implement **type-safe Jetpack Compose Navigation** across multiple Gradle modules using **Hilt** for dependency injection — without any module depending directly on another feature module's internals.

**Repo:** [androidwithabhishek/Navigation-In-Multimodule-Application-](https://github.com/androidwithabhishek/Navigation-In-Multimodule-Application-)

---

## 📱 Screenshots

| Auth Screen | Dashboard Screen |
|---|---|
| ![Auth Screen](https://raw.githubusercontent.com/androidwithabhishek/my-res/main/multi_module_app/screenshots/auth_screen.jpeg) | ![Dashboard Screen](https://raw.githubusercontent.com/androidwithabhishek/my-res/main/multi_module_app/screenshots/dashboard_screen.jpeg) |

---

## 🏗️ Module Structure

```
MultiModuleNavigation
├── app                     # Hosts the single NavHost, wires everything together
├── common                  # Shared navigation contracts (no feature knows about another feature)
│   ├── SubGraph.kt
│   ├── MainGraph.kt
│   └── NavigationFeatureApi.kt
└── feature
    ├── auth                # Auth feature module (self-contained)
    └── desboard            # Dashboard feature module (self-contained)
```

**Dependency direction:** `app → feature:*` and `feature:* → common`. Feature modules **never** depend on each other — they only know about `common`. This is the whole trick that keeps the graph scalable.

---

## 🧩 The Core Idea

Instead of one giant `NavHost` that knows about every screen in every feature, each feature module exposes a **contract** (`NavigationFeatureApi`) that says *"give me a NavHostController and a NavGraphBuilder, and I'll register my own destinations."* The `app` module just collects these contracts via Hilt and calls them.

### 1. Shared contract (`common` module)

```kotlin
// common/NavigationFeatureApi.kt
interface NavigationFeatureApi {
    fun registerGraph(navHostController: NavHostController, navGraphBuilder: NavGraphBuilder)
}
```

### 2. Type-safe destinations (`common` module)

Using Kotlin Serialization instead of string routes:

```kotlin
// common/SubGraph.kt  -> top-level nested graphs
sealed class SubGraph {
    @Serializable data object Auth : SubGraph()
    @Serializable data object DesBoard : SubGraph()
}

// common/MainGraph.kt -> actual screens inside those graphs
sealed class MainGraph {
    @Serializable data object AuthScreen : MainGraph()
    @Serializable data object DesboardScreen : MainGraph()
}
```

### 3. Each feature implements the contract for itself

```kotlin
// feature/auth/AuthNavigation.kt
interface AuthNavigationFeatureApi : NavigationFeatureApi

class AuthNavigationFeatureApiImpl : AuthNavigationFeatureApi {
    override fun registerGraph(
        navHostController: NavHostController,
        navGraphBuilder: NavGraphBuilder,
    ) {
        navGraphBuilder.navigation<SubGraph.Auth>(startDestination = MainGraph.AuthScreen) {
            composable<MainGraph.AuthScreen> {
                AuthScreen(
                    onBackClick = { navHostController.popBackStack() },
                    onGoDesClick = { navHostController.navigate(MainGraph.DesboardScreen) }
                )
            }
        }
    }
}
```

The Dashboard module mirrors the exact same pattern independently:

```kotlin
// feature/desboard/DesboardNavigation.kt
class DesboardNavigationFeatureApiImpl : DesboardNavigationFeatureApi {
    override fun registerGraph(
        navHostController: NavHostController,
        navGraphBuilder: NavGraphBuilder,
    ) {
        navGraphBuilder.navigation<SubGraph.DesBoard>(startDestination = MainGraph.DesboardScreen) {
            composable<MainGraph.DesboardScreen> {
                DashboardScreen(
                    onBackClick = { navHostController.popBackStack() },
                    onGoAuthClick = { navHostController.navigate(MainGraph.AuthScreen) }
                )
            }
        }
    }
}
```

### 4. Hilt binds each implementation to its interface

```kotlin
// feature/auth/di/HiltModule.kt
@InstallIn(SingletonComponent::class)
@Module
object HiltModule {
    @Provides
    @Singleton
    fun providesAuthNavigationFeatureApi(): AuthNavigationFeatureApi =
        AuthNavigationFeatureApiImpl()
}
```

The `desboard` module has an identical Hilt module binding `DesboardNavigationFeatureApi`.

### 5. `app` collects all feature APIs into one navigator

```kotlin
// app/ui/DefaultNavigator.kt
data class DefaultNavigator(
    val authNavigationFeatureApi: AuthNavigationFeatureApi,
    val desboardNavigationFeatureApi: DesboardNavigationFeatureApi,
)

// app/di/HiltModule.kt
@Provides
fun providesDefaultNavigator(
    authNavigationFeatureApi: AuthNavigationFeatureApi,
    desboardNavigationFeatureApi: DesboardNavigationFeatureApi,
): DefaultNavigator = DefaultNavigator(authNavigationFeatureApi, desboardNavigationFeatureApi)
```

### 6. `MainActivity` builds the single `NavHost`

```kotlin
// app/MainActivity.kt
@AndroidEntryPoint
class MainActivity : ComponentActivity() {

    @Inject
    lateinit var defaultNavigator: DefaultNavigator

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MultiModuleNavigationTheme {
                val navController = rememberNavController()
                MainNavigation(
                    navHostController = navController,
                    defaultNavigator = defaultNavigator
                )
            }
        }
    }
}

@Composable
fun MainNavigation(
    modifier: Modifier = Modifier,
    navHostController: NavHostController,
    defaultNavigator: DefaultNavigator,
) {
    NavHost(navHostController, startDestination = SubGraph.Auth) {
        defaultNavigator.authNavigationFeatureApi.registerGraph(navHostController, this)
        defaultNavigator.desboardNavigationFeatureApi.registerGraph(navHostController, this)
    }
}
```

Notice `app` **never imports `AuthScreen` or `DashboardScreen` directly** — it only depends on the `*NavigationFeatureApi` interfaces from `common`, and Hilt supplies the concrete implementations at runtime.

---

## ⚙️ Module Wiring (`settings.gradle.kts`)

```kotlin
rootProject.name = "MultiModuleNavigation"
include(":app")
include(":common")
include(":feature:auth")
include(":feature:desboard")
```

Dependencies in `app/build.gradle.kts`:

```kotlin
dependencies {
    implementation(project(":common"))
    implementation(project(":feature:desboard"))
    implementation(project(":feature:auth"))

    implementation(libs.hilt.android)
    ksp(libs.hilt.compiler)
    implementation(libs.androidx.hilt.navigation.compose)
}
```

---

## ✅ Why this pattern is worth using

- **Decoupled features** – `auth` and `desboard` have zero compile-time knowledge of each other.
- **Type-safe navigation** – routes are `@Serializable` sealed classes, not error-prone string literals.
- **Scales cleanly** – adding a new feature module means: add a `NavigationFeatureApi`, implement it, bind it with Hilt, plug it into `DefaultNavigator`. No changes needed inside other features.
- **Single source of truth for the graph** – `app` owns the one and only `NavHost`, while each feature owns its own sub-graph and screens.

---

## 🛠️ Tech Stack

- Kotlin + Jetpack Compose
- Navigation Compose (type-safe routes via `kotlinx.serialization`)
- Dagger Hilt (multi-module DI)
- Gradle multi-module setup (`app`, `common`, `feature:auth`, `feature:desboard`)