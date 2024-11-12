# 12. Cycle de vie et architecture

## Cycle de vie général d'une application Android

### Concepts fondamentaux

Une application Android est composée de différents composants qui ont chacun leur propre cycle de vie, mais l'élément
principal est l'**Activité** (`Activity`)[1]. Une activité représente un écran de l'application avec lequel
l'utilisateur peut interagir[1].

Le cycle de vie d'une activité s'étend de sa création à sa destruction, moment où le système récupère ses ressources[1].
Comprendre ce cycle de vie est essentiel pour :

- Éviter les crashs lors des interruptions (appels téléphoniques, changement d'application)
- Optimiser l'utilisation des ressources système
- Préserver les données et l'état de l'application
- Gérer correctement les changements de configuration (rotation d'écran)[10]

### États principaux d'une activité

Une activité peut se trouver dans quatre états principaux[14] :

- **Active/Resumed** : L'activité est au premier plan et interactive
- **En pause** : Visible mais a perdu le focus
- **Stoppée** : Non visible mais conservée en mémoire
- **Détruite** : L'activité est terminée

![Cycle de vie](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-activity-lifecycle/img/468988518c270b38.png?hl=fr)

### Méthodes du cycle de vie

Les principales méthodes appelées lors des transitions entre états sont[10] :

1. `onCreate()` : Initialisation de l'activité
2. `onStart()` : L'activité devient visible
3. `onResume()` : L'activité devient interactive
4. `onPause()` : L'activité perd le focus
5. `onStop()` : L'activité n'est plus visible
6. `onDestroy()` : L'activité est détruite

## Particularités avec Jetpack Compose

### Approche déclarative

Jetpack Compose utilise une approche différente du cycle de vie traditionnel[2]. Au lieu de gérer directement les
changements d'état via des méthodes de callback, Compose utilise un paradigme déclaratif[8].

### Gestion de l'état

Dans Compose, le cycle de vie est étroitement lié à la gestion de l'état[12] :

- Les composables sont sans état (stateless) ou avec état (stateful)
- L'état est géré via des objets `State<T>` et la fonction `remember{}`
- La recomposition se produit automatiquement lors des changements d'état

### Effets et cycle de vie

Compose introduit des effets spécifiques pour gérer les opérations liées au cycle de vie[12] :

```kotlin
@Composable
fun MonComposable() {
    // Effet exécuté à chaque recomposition
    LaunchedEffect(key1) {
        // Code à exécuter
    }

    // Effet exécuté uniquement à la première composition
    DisposableEffect(key1) {
        onDispose {
            // Nettoyage des ressources
        }
    }
}
```

### Bonnes pratiques

Pour une application Compose efficace[4] :

- Privilégier les composants sans état (stateless)
- Élever l'état au niveau approprié
- Utiliser les effets pour les opérations ayant des effets secondaires
- Éviter les effets secondaires dans les composables
- Gérer correctement la recomposition pour optimiser les performances

En comprenant ces concepts, vous pourrez développer des applications Android robustes qui gèrent correctement leur cycle
de vie, que ce soit avec l'approche traditionnelle ou avec Jetpack Compose.

Citations:
[1] https://developer.android.com/codelabs/basic-android-kotlin-compose-activity-lifecycle?hl=fr
[2] https://appmaster.io/fr/blog/jetpack-compose-un-guide-du-debutant
[3] https://www.weblineindia.com/fr/blog/android-app-development-lifecycle.html
[4] https://appmaster.io/fr/blog/kotlin-avec-jetpack-compose-les-meilleures-pratiques
[5] http://www.iro.umontreal.ca/~dift1155/cours/ift1155/communs/Cours/2P/C02_CycledeVie_2P.pdf
[6] https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=fr
[7] https://developer.android.com/codelabs/basic-android-kotlin-training-activity-lifecycle?hl=fr
[8] https://developer.android.com/develop/ui/compose/state?hl=fr
[9] https://www.yeeply.com/fr/blog/developpement-applications-mobiles/cycle-de-vie-developpement-logiciels-mobiles/
[10] https://developer.android.com/guide/components/activities/activity-lifecycle?hl=fr
[11] https://openclassrooms.com/fr/courses/8150246-developpez-votre-premiere-application-android/8256687-apprehendez-le-cycle-de-vie-d-une-application
[12] https://www.editions-eni.fr/livre/jetpack-compose-developpez-des-interfaces-accessibles-et-modernes-pour-android-9782409039669/gestion-des-etats-et-des-effets
[13] https://www.lirmm.fr/~fmichel/ens/android/cours/Android_lifecycle.pdf
[14] https://mathias-seguy.developpez.com/tutoriels/android/comprendre-cyclevie-activite/


Je vais détailler l'architecture moderne recommandée pour une application Android utilisant Jetpack Compose.

## Architecture en couches

Une application Android moderne avec Jetpack Compose devrait suivre une architecture en couches distinctes :

### UI Layer (Présentation)

La couche UI est responsable d'afficher les données et de gérer les interactions utilisateur. Avec Jetpack Compose, elle comprend :

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

### Domain Layer (Optionnelle)

Cette couche intermédiaire contient la logique métier et fait le lien entre UI et Data :

- Use cases
- Modèles de domaine
- Business logic

```kotlin
class GetUserUseCase(
    private val userRepository: UserRepository
) {
    suspend operator fun invoke(userId: String): User {
        return userRepository.getUser(userId)
    }
}
```

### Data Layer

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

- State hoisting
- ViewModel avec StateFlow/SharedFlow
- remember/rememberSaveable

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

La navigation est gérée par Navigation Compose :

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

Hilt est recommandé pour l'injection de dépendances :

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