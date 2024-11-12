# 16. Intentions vs. navigation

## `Intent` vs. `Navigation`

### Navigation Compose

#### Avantages

- **Sécurité de type** : Vérifications à la compilation, réduisant les erreurs potentielles[8]
- **Intégration native** avec Jetpack Compose et l'architecture moderne d'Android[8]
- **Code plus lisible et maintenable**, moins de code répétitif[8]
- **Approche déclarative** qui simplifie la navigation[8]

#### Inconvénients

- Limité aux types de données de base pour les arguments de navigation[6]
- Nécessite l'ajout d'une dépendance supplémentaire[5]

### Navigation par Intent

#### Avantages

- **Flexibilité** : Utilisable dans n'importe quel projet Android[8]
- **Familiarité** : Documentation extensive et support communautaire[8]
- **Capacité à passer des objets Parcelable** facilement[6]
- **Intégration facile** avec d'autres composants Android[8]

#### Inconvénients

- **Pas de vérification à la compilation** : Risques d'erreurs à l'exécution[8]
- **Plus verbeux** : Nécessite plus de code pour la gestion de la navigation[8]
- **Gestion manuelle** du cycle de vie et de l'état[8]

### Recommandation

Pour une nouvelle application utilisant Jetpack Compose :

- Privilégier la **Navigation Compose** pour la navigation interne à l'application[8]
- Utiliser les **Intents** uniquement pour :
    - La navigation vers d'autres applications
    - Les cas nécessitant le passage d'objets complexes
    - L'intégration avec des composants Android traditionnels

Cette approche hybride permet de bénéficier des avantages de la Navigation Compose tout en gardant la flexibilité des
Intents quand nécessaire[8].

Citations:
[1] https://stackoverflow.com/questions/65088035/how-to-navigate-from-a-composable-to-an-activity-in-jetpack-compose
[2] https://guides.peruzal.com/v1/android-guides/navigation/intents/
[3] https://www.geeksforgeeks.org/start-a-new-activity-using-intent-in-android-using-jetpack-compose/
[4] https://developer.android.com/develop/ui/compose/navigation
[5] https://developer.android.com/develop/ui/compose/navigation?hl=fr
[6] https://betterprogramming.pub/intent-based-compose-navigation-1087634b984a
[7] https://blog.kotlin-academy.com/mastery-navigation-in-jetpack-compose-db00b0a0ef75?gi=007e50484ede
[8] https://rommansabbir.com/typesafe-navigation-or-traditional-intent-passing