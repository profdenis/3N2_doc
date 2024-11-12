# 13. Architecture

## Architecture en couches

Une application Android moderne avec Jetpack Compose devrait suivre une architecture en couches distinctes :

### UI Layer (Présentation)

La couche UI est responsable d'afficher les données et de gérer les interactions utilisateur. Avec Jetpack Compose, elle
comprend :

**Composants principaux :**

- Composables UI
- ViewModels
- UI State
- UI Events

```kotlin
@Composable
fun MyScreen(
    viewModel: MyViewModel = viewModel()
) {
    val uiState by viewModel.uiState.collectAsState()

    Column {
        // UI elements
    }
}
```

### Couche du domaine (Domain Layer) (Optionnelle)

Cette couche intermédiaire contient la logique d'affaires (logique métier, *business logic*) et fait le lien entre UI et
Data :

- Cas d'utilisation
- Modèles de domaine
- Logique d'affaires

```kotlin
class GetUserUseCase(
    private val userRepository: UserRepository
) {
    suspend operator fun invoke(userId: String): User {
        return userRepository.getUser(userId)
    }
}
```

### Couche des données (Data Layer)

Gère les données de l'application :

- Repositories
- Data sources (API, base de données)
- Modèles de données

```kotlin
class UserRepository(
    private val api: ApiService,
    private val database: AppDatabase
) {
    suspend fun getUser(id: String): User {
        return database.userDao().getUser(id)
            ?: api.fetchUser(id).also { user ->
                database.userDao().insert(user)
            }
    }
}
```

## Gestion de l'état avec Compose

### État UI

L'état UI est géré via :

- L'élévation de l'état (*State hoisting*)
- `ViewModel` avec `StateFlow`/`SharedFlow`
- `remember`/`rememberSaveable`

```kotlin
class MyViewModel : ViewModel() {
    private val _uiState = MutableStateFlow(MyUiState())
    val uiState: StateFlow<MyUiState> = _uiState.asStateFlow()

    fun handleEvent(event: MyUiEvent) {
        // Update state based on events
    }
}
```

### Effets secondaires

Les effets sont gérés avec les API Compose :

```kotlin
@Composable
fun MyScreen() {
    LaunchedEffect(key1) {
        // One-time setup
    }

    DisposableEffect(key1) {
        onDispose {
            // Cleanup
        }
    }
}
```

## Navigation

La navigation est gérée par *Navigation Compose* :

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    NavHost(navController, startDestination = "home") {
        composable("home") { HomeScreen() }
        composable("details/{id}") { backStackEntry ->
            DetailsScreen(backStackEntry.arguments?.getString("id"))
        }
    }
}
```

## Injection de dépendances

*Hilt* est recommandé pour l'injection de dépendances :

```kotlin
@HiltViewModel
class MainViewModel @Inject constructor(
    private val userRepository: UserRepository
) : ViewModel()
```

## Bonnes pratiques

1. **Unidirectional Data Flow (UDF)**
   - Les événements UI remontent vers le ViewModel
   - L'état descend vers les composables UI

2. **State Hoisting**
   - Séparer l'état de sa manipulation
   - Remonter l'état au niveau approprié

3. **Single Source of Truth**
   - Une seule source de vérité pour les données
   - Utilisation de Flow pour la réactivité

4. **Composables sans état**
   - Préférer les composables sans état quand possible
   - Séparer la logique de l'affichage

5. **Modularisation**
   - Diviser l'application en modules fonctionnels
   - Utiliser les feature modules pour une meilleure scalabilité

Cette architecture permet de créer des applications maintenables, testables et évolutives avec Jetpack Compose.

Citations:
[1] https://www.geeksforgeeks.org/android-architecture/
[2] https://www.simform.com/blog/mobile-application-architecture/
[3] https://www.intelivita.com/blog/android-architecture-patterns/
[4] https://developer.android.com/topic/architecture?hl=en
[5] https://w3r.one/fr/blog/mobile/android/architecture-android/comprendre-architecture-android-vie-ensemble-composants-modeles
[6] https://www.zucisystems.com/be/blog/limportance-de-larchitecture-mobile-concevoir-des-applications-pour-reussir/
[7] https://appmaster.io/fr/blog/kotlin-avec-jetpack-compose-les-meilleures-pratiques
[8] https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=fr
