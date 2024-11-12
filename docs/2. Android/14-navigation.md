# 14. Navigation dans Jetpack Compose

[Dépôt Git](https://github.com/profdenis/NavigationSimple)

## Exemple avec des boutons pour naviguer entre les composables

### Configuration initiale

Ajoutez d'abord la dépendance dans le fichier `build.gradle` :

```kotlin
dependencies {
    implementation("androidx.navigation:navigation-compose:2.8.3")
}
```

### Structure de l'exemple

Nous allons créer une application avec trois écrans :

- Écran d'accueil
- Écran de profil
- Écran de paramètres

#### Définition des routes

```kotlin
sealed class Screen(val route: String) {
    object Home : Screen("home")
    object Profile : Screen("profile")
    object Settings : Screen("settings")
}
```

#### Création des composables d'écran

```kotlin
@Composable
fun HomeScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Écran d'accueil")
        Button(onClick = { navController.navigate(Screen.Profile.route) }) {
            Text("Aller au profil")
        }
        Button(onClick = { navController.navigate(Screen.Settings.route) }) {
            Text("Aller aux paramètres")
        }
    }
}

@Composable
fun ProfileScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Écran de profil")
        Button(onClick = { navController.navigate(Screen.Settings.route) }) {
            Text("Aller aux paramètres")
        }
        Button(onClick = { navController.popBackStack() }) {
            Text("Retour")
        }
    }
}

@Composable
fun SettingsScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Écran des paramètres")
        Button(onClick = { navController.popBackStack() }) {
            Text("Retour")
        }
        Button(
            onClick = {
                navController.navigate(Screen.Home.route) {
                    popUpTo(Screen.Home.route) { inclusive = true }
                }
            }
        ) {
            Text("Retour à l'accueil")
        }
    }
}
```

#### Configuration de la navigation

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    NavHost(
        navController = navController,
        startDestination = Screen.Home.route
    ) {
        composable(Screen.Home.route) {
            HomeScreen(navController)
        }
        composable(Screen.Profile.route) {
            ProfileScreen(navController)
        }
        composable(Screen.Settings.route) {
            SettingsScreen(navController)
        }
    }
}
```

#### Intégration dans MainActivity

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MonTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    AppNavigation()
                }
            }
        }
    }
}
```

### Fonctionnalités démontrées

1. **Navigation de base** : Navigation entre les écrans avec `navigate()`[1]
2. **Navigation arrière** : Utilisation de `popBackStack()`[2]
3. **Navigation avec effacement de pile** : Utilisation de `popUpTo` pour retourner à l'accueil[2]

### Points importants

- Le `NavController` gère l'état de la navigation[1]
- Le `NavHost` définit le graphe de navigation[1][2]
- Chaque écran reçoit le `NavController` pour gérer la navigation[3]
- `popBackStack()` permet de revenir à l'écran précédent[2]
- `popUpTo` avec `inclusive = true` permet de remonter jusqu'à une destination en l'incluant dans la suppression[5]

Cet exemple montre les bases de la navigation dans Jetpack Compose tout en restant simple et compréhensible pour des
débutants.

Citations:
[1] https://proandroiddev.com/android-jetpack-compose-navigation-1cdfc488b891?gi=d31e0323b815
[2] https://saurabhjadhavblogs.com/ultimate-guide-to-jetpack-compose-navigation
[3] https://blog.kotlin-academy.com/mastery-navigation-in-jetpack-compose-db00b0a0ef75?gi=007e50484ede
[4] https://developer.android.com/develop/ui/compose/navigation
[5] https://proandroiddev.com/mastering-navigation-in-jetpack-compose-a-guide-to-using-the-inclusive-attribute-b66916a5f15c?gi=401071494588
[6] https://developer.android.com/codelabs/basic-android-kotlin-compose-navigation
[7] https://developer.android.com/develop/ui/compose/navigation?hl=fr
[8] https://www.youtube.com/watch?v=AIC_OFQ1r3k

## Exemple avec un menu dans la barre supérieure de l'application

### Structure de l'application

```kotlin
sealed class Screen(val route: String, val title: String) {
    object Home : Screen("home", "Accueil")
    object Profile : Screen("profile", "Profil")
    object Settings : Screen("settings", "Paramètres")
}
```

### Configuration de la navigation

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    Scaffold(
        topBar = {
            TopAppBarWithMenu(navController)
        }
    ) { paddingValues ->
        NavHost(
            navController = navController,
            startDestination = Screen.Home.route,
            modifier = Modifier.padding(paddingValues)
        ) {
            composable(Screen.Home.route) {
                HomeScreen()
            }
            composable(Screen.Profile.route) {
                ProfileScreen()
            }
            composable(Screen.Settings.route) {
                SettingsScreen()
            }
        }
    }
}
```

### TopBar avec menu

```kotlin
@Composable
fun TopAppBarWithMenu(navController: NavController) {
    var showMenu by remember { mutableStateOf(false) }

    TopAppBar(
        title = { Text("Mon Application") },
        actions = {
            IconButton(onClick = { showMenu = !showMenu }) {
                Icon(Icons.Default.MoreVert, contentDescription = "Menu")
            }
            DropdownMenu(
                expanded = showMenu,
                onDismissRequest = { showMenu = false }
            ) {
                DropdownMenuItem(
                    text = { Text(Screen.Home.title) },
                    onClick = {
                        navController.navigate(Screen.Home.route) {
                            popUpTo(Screen.Home.route) { inclusive = true }
                        }
                        showMenu = false
                    }
                )
                DropdownMenuItem(
                    text = { Text(Screen.Profile.title) },
                    onClick = {
                        navController.navigate(Screen.Profile.route)
                        showMenu = false
                    }
                )
                DropdownMenuItem(
                    text = { Text(Screen.Settings.title) },
                    onClick = {
                        navController.navigate(Screen.Settings.route)
                        showMenu = false
                    }
                )
            }
        }
    )
}
```

### Écrans de l'application

```kotlin
@Composable
fun HomeScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Écran d'accueil")
    }
}

@Composable
fun ProfileScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Écran de profil")
    }
}

@Composable
fun SettingsScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Écran des paramètres")
    }
}
```

### Points importants à noter

1. Le menu est géré par un état `showMenu` qui contrôle l'affichage du `DropdownMenu`

2. Chaque item du menu utilise `navController.navigate()` pour la navigation

3. Pour l'écran d'accueil, nous utilisons `popUpTo` avec `inclusive = true` pour éviter l'accumulation d'écrans dans la
   pile de navigation

4. Le `Scaffold` gère automatiquement le padding nécessaire pour éviter que le contenu ne soit caché derrière la TopBar

5. Les écrans sont simples mais peuvent être enrichis selon les besoins de l'application

Cette implémentation offre une navigation claire et intuitive via un menu déroulant dans la barre supérieure de
l'application.

Citations:
[1] https://saurabhjadhavblogs.com/ultimate-guide-to-jetpack-compose-navigation
[2] https://proandroiddev.com/android-jetpack-compose-navigation-1cdfc488b891?gi=d31e0323b815
[3] https://proandroiddev.com/implement-bottom-bar-navigation-in-jetpack-compose-b530b1cd9ee2?gi=5c1ab6e9d027
[4] https://proandroiddev.com/mastering-navigation-in-jetpack-compose-a-guide-to-using-the-inclusive-attribute-b66916a5f15c?gi=401071494588
[5] https://blog.kotlin-academy.com/mastery-navigation-in-jetpack-compose-db00b0a0ef75?gi=007e50484ede
[6] https://developer.android.com/codelabs/basic-android-kotlin-compose-navigation
[7] https://www.youtube.com/watch?v=JLICaBEiJS0
[8] https://developer.android.com/develop/ui/compose/navigation

## Exemple avec des icônes dans la barre inférieure

### Structure de l'application

```kotlin
sealed class Screen(
    val route: String,
    val title: String,
    val icon: ImageVector
) {
    object Home : Screen(
        route = "home",
        title = "Accueil",
        icon = Icons.Default.Home
    )
    object Profile : Screen(
        route = "profile",
        title = "Profil",
        icon = Icons.Default.Person
    )
    object Settings : Screen(
        route = "settings",
        title = "Paramètres",
        icon = Icons.Default.Settings
    )

    companion object {
        val items = listOf(Home, Profile, Settings)
    }
}
```

### Configuration de la navigation

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()
    val navBackStackEntry by navController.currentBackStackEntryAsState()
    val currentRoute = navBackStackEntry?.destination?.route

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Mon Application") }
            )
        },
        bottomBar = {
            NavigationBar {
                Screen.items.forEach { screen ->
                    NavigationBarItem(
                        icon = {
                            Icon(screen.icon, contentDescription = screen.title)
                        },
                        label = { Text(screen.title) },
                        selected = currentRoute == screen.route,
                        onClick = {
                            navController.navigate(screen.route) {
                                // Évite l'empilement des destinations
                                popUpTo(navController.graph.startDestinationId) {
                                    saveState = true
                                }
                                // Évite les copies multiples de la même destination
                                launchSingleTop = true
                                // Restaure l'état lors de la reselection
                                restoreState = true
                            }
                        }
                    )
                }
            }
        }
    ) { paddingValues ->
        NavHost(
            navController = navController,
            startDestination = Screen.Home.route,
            modifier = Modifier.padding(paddingValues)
        ) {
            composable(Screen.Home.route) {
                HomeScreen()
            }
            composable(Screen.Profile.route) {
                ProfileScreen()
            }
            composable(Screen.Settings.route) {
                SettingsScreen()
            }
        }
    }
}
```

### Écrans de l'application

```kotlin
@Composable
fun HomeScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Icon(
            Icons.Default.Home,
            contentDescription = null,
            modifier = Modifier.size(48.dp)
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            "Écran d'accueil",
            style = MaterialTheme.typography.headlineMedium
        )
    }
}

@Composable
fun ProfileScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Icon(
            Icons.Default.Person,
            contentDescription = null,
            modifier = Modifier.size(48.dp)
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            "Écran de profil",
            style = MaterialTheme.typography.headlineMedium
        )
    }
}

@Composable
fun SettingsScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Icon(
            Icons.Default.Settings,
            contentDescription = null,
            modifier = Modifier.size(48.dp)
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            "Écran des paramètres",
            style = MaterialTheme.typography.headlineMedium
        )
    }
}
```

### Points importants

1. La `NavigationBar` utilise `NavigationBarItem` pour chaque destination

2. L'état `selected` est géré en comparant la route courante avec la route de l'item

3. La navigation inclut des options importantes :
    - `launchSingleTop` évite les copies multiples
    - `popUpTo` avec `saveState` gère correctement la pile de navigation
    - `restoreState` préserve l'état lors de la reselection

4. Les icônes et les titres sont définis dans la classe scellée `Screen`

5. Le `currentBackStackEntryAsState()` permet de suivre la destination actuelle

Cette implémentation offre une navigation fluide et intuitive avec une barre de navigation inférieure, couramment
utilisée dans les applications mobiles modernes. Les utilisateurs peuvent facilement basculer entre les différentes
sections de l'application en touchant les icônes correspondantes.
