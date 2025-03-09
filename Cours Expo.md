# Cours Complet sur Expo et React Native

## Table des matières

1. [Introduction à Expo et React Native](#introduction-à-expo-et-react-native)
2. [Création d'un projet Expo](#création-dun-projet-expo)
3. [Configuration de l'environnement de développement](#configuration-de-lenvironnement-de-développement)
4. [Metro Bundler](#metro-bundler)
5. [Expo Go vs Development Build](#expo-go-vs-development-build)
6. [Structure d'un projet Expo](#structure-dun-projet-expo)
7. [Composants fondamentaux de React Native](#composants-fondamentaux-de-react-native)
8. [SafeArea et considérations de mise en page](#safearea-et-considérations-de-mise-en-page)
9. [Expo Router](#expo-router)
10. [Gestion d'état](#gestion-détat)
11. [Utilisation des API Expo](#utilisation-des-api-expo)
12. [Styles et mise en page](#styles-et-mise-en-page)
13. [Tests et débogage](#tests-et-débogage)
14. [Publication et déploiement](#publication-et-déploiement)
15. [Expo EAS (Expo Application Services)](#expo-eas-expo-application-services)
16. [Bonnes pratiques](#bonnes-pratiques)
17. [Ressources et liens utiles](#ressources-et-liens-utiles)

## Introduction à Expo et React Native

### Qu'est-ce que React Native?

React Native est un framework open-source développé par Meta (anciennement Facebook) qui permet de créer des applications mobiles natives pour iOS et Android en utilisant JavaScript et React. Contrairement aux applications hybrides, React Native compile votre code JavaScript en composants natifs, offrant ainsi des performances proches des applications développées en Swift (iOS) ou Kotlin/Java (Android).

**Principes clés de React Native:**

- **Écrire une fois, déployer partout**: Un seul code base pour iOS et Android
- **Composants natifs**: Utilise les mêmes composants fondamentaux que les applications natives
- **Rechargement à chaud (Hot Reloading)**: Permet de voir instantanément les modifications apportées au code
- **Architecture basée sur React**: Utilise le même paradigme de programmation déclarative que React

### Qu'est-ce qu'Expo?

Expo est une plateforme et un ensemble d'outils construits autour de React Native qui simplifient considérablement le développement, le test et le déploiement d'applications React Native. Expo fournit:

- Un ensemble d'API JavaScript pour accéder aux fonctionnalités natives des appareils
- Expo Go pour tester rapidement vos applications sur des appareils réels
- Des outils de construction (build) et de publication via EAS (Expo Application Services)
- Une solution complète pour gérer le cycle de vie des applications mobiles
- Expo Router pour une navigation moderne basée sur le système de fichiers

### L'écosystème Expo en 2025

Expo a considérablement évolué depuis ses débuts. Voici les éléments clés de l'écosystème Expo actuel:

1. **Expo SDK**: Bibliothèque de packages et d'API qui facilitent l'accès aux fonctionnalités natives

2. **Expo Go**: Application permettant de tester rapidement des projets sans avoir à construire des builds natifs

3. **Development Builds**: Builds personnalisés de votre application incluant les outils de développement Expo

4. **Expo Router**: Système de navigation moderne basé sur le système de fichiers, similaire à Next.js

5. **EAS (Expo Application Services)**: 
   - **EAS Build**: Service de build pour créer des applications natives
   - **EAS Submit**: Service pour soumettre des applications aux App Stores
   - **EAS Update**: Mises à jour over-the-air sans passer par les stores

6. **Metro Bundler**: Le bundler JavaScript utilisé pour empaqueter l'application React Native

### Modes de développement Expo

Expo propose deux principaux paradigmes de développement:

1. **Workflow Managed (Expo Go)**: 
   - Idéal pour les débutants et les tests rapides
   - Limitations pour les modules natifs personnalisés
   - Facile à mettre en place et à partager

2. **Développement avec Development Builds**:
   - Recommandé pour les projets de production
   - Support complet des modules natifs personnalisés
   - Plus flexible mais nécessite plus de configuration

> **Note importante**: Expo a considérablement simplifié le processus de développement au fil des ans. Les anciennes distinctions entre "managed workflow" et "bare workflow" sont maintenant moins pertinentes, avec un focus sur Expo Go vs Development Builds.

## Création d'un projet Expo

La création d'un projet Expo est maintenant plus simple que jamais. La méthode recommandée utilise `create-expo-app` directement, sans avoir besoin d'installer la CLI Expo globalement.

### Prérequis

Avant de commencer, assurez-vous d'avoir installé:

- **Node.js** (version LTS recommandée, 18.x ou plus récente)
- **Git** (recommandé pour la gestion de version)
- Un éditeur de code (comme Visual Studio Code)

### Création d'un nouveau projet avec create-expo-app

Pour créer un nouveau projet Expo:

```bash
# Méthode recommandée (sans options)
npx create-expo-app@latest

# Avec un nom spécifique
npx create-expo-app@latest mon-app-expo
```

Lorsque vous exécutez cette commande, vous serez guidé à travers différentes options pour configurer votre projet.

### Templates disponibles

Vous pouvez créer un projet à partir de différents templates pour démarrer plus rapidement:

```bash
# Avec TypeScript
npx create-expo-app@latest --template expo-template-blank-typescript

# Avec Expo Router
npx create-expo-app@latest --template expo-router

# Avec Tabs de navigation (également avec Expo Router)
npx create-expo-app@latest --template tabs
```

Les templates les plus populaires sont:

| Template | Description |
|----------|-------------|
| `blank` | Template minimal avec JavaScript |
| `blank-typescript` | Template minimal avec TypeScript |
| `expo-router` | Projet avec Expo Router configuré |
| `tabs` | Projet avec Expo Router et navigation par onglets |

### Exemple de création de projet avec Expo Router

```bash
# Créer un projet avec Expo Router et TypeScript
npx create-expo-app@latest mon-app --template expo-router --language typescript
```

### Démarrage du projet

Une fois votre projet créé, vous pouvez le démarrer:

```bash
# Naviguer dans le dossier du projet
cd mon-app

# Démarrer le projet
npx expo start
```

Cette commande démarre le serveur de développement Metro et affiche un QR code que vous pouvez scanner avec l'application Expo Go (sur Android) ou l'appareil photo (sur iOS) pour ouvrir votre application.

## Configuration de l'environnement de développement

Pour développer avec Expo, vous devez configurer votre environnement en fonction de la plateforme cible et de votre méthode de développement préférée.

### Développement sur appareil physique (recommandé)

Le développement sur un appareil physique offre la meilleure expérience, car vous voyez exactement ce que vos utilisateurs verront.

#### Pour Android

1. **Installez l'application Expo Go** sur votre appareil Android depuis le [Google Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent)

2. **Assurez-vous que votre appareil et votre ordinateur sont sur le même réseau Wi-Fi**

3. **Démarrez votre projet**:
   ```bash
   npx expo start
   ```

4. **Scannez le QR code** affiché dans le terminal avec l'application Expo Go

#### Pour iOS

1. **Installez l'application Expo Go** sur votre appareil iOS depuis l'[App Store](https://apps.apple.com/app/apple-store/id982107779)

2. **Assurez-vous que votre appareil et votre ordinateur sont sur le même réseau Wi-Fi**

3. **Démarrez votre projet**:
   ```bash
   npx expo start
   ```

4. **Scannez le QR code** affiché dans le terminal avec l'appareil photo iOS, qui vous dirigera vers Expo Go

### Développement sur émulateur/simulateur

Si vous n'avez pas d'appareil physique, vous pouvez utiliser des émulateurs ou simulateurs.

#### Android Emulator

1. **Installez Android Studio** depuis le [site officiel](https://developer.android.com/studio)

2. **Configurez un émulateur**:
   - Ouvrez Android Studio
   - Cliquez sur "More Actions" > "Virtual Device Manager"
   - Cliquez sur "Create Device" et suivez les instructions
   - Choisissez une image système avec Google Play Store

3. **Démarrez l'émulateur** depuis le Virtual Device Manager

4. **Lancez votre application sur l'émulateur**:
   ```bash
   npx expo start
   ```
   Puis appuyez sur `a` dans le terminal ou cliquez sur "Run on Android device/emulator"

#### iOS Simulator (macOS uniquement)

1. **Installez Xcode** depuis l'App Store

2. **Installez le simulateur iOS**:
   - Ouvrez Xcode
   - Allez dans Xcode > Preferences > Components
   - Téléchargez un simulateur iOS (le plus récent est recommandé)

3. **Lancez votre application sur le simulateur**:
   ```bash
   npx expo start
   ```
   Puis appuyez sur `i` dans le terminal ou cliquez sur "Run on iOS simulator"

### Interface de développement d'Expo

Lorsque vous démarrez votre projet avec `npx expo start`, plusieurs options s'offrent à vous:

- **QR Code**: À scanner avec Expo Go
- **Interface Web**: Accessible via votre navigateur pour contrôler votre session de développement
- **Commandes du terminal**:
  - Appuyez sur `a` pour ouvrir sur Android
  - Appuyez sur `i` pour ouvrir sur iOS
  - Appuyez sur `w` pour ouvrir dans un navigateur web
  - Appuyez sur `r` pour recharger l'application
  - Appuyez sur `m` pour basculer le menu de développement sur l'appareil
  - Appuyez sur `d` pour ouvrir l'interface de débogage

### Debugging

Pour un débogage efficace:

1. **Secouez votre appareil** ou appuyez sur `Cmd+D` (iOS) / `Ctrl+M` (Android) pour ouvrir le menu de développement

2. **Options de débogage disponibles**:
   - Reload: Recharge l'application
   - Debug Remote JS: Ouvre le débogueur JavaScript dans Chrome
   - Toggle Inspector: Active l'inspecteur d'éléments
   - Performance Monitor: Affiche les performances de l'application

3. **React DevTools**: Pour une expérience de débogage plus avancée, vous pouvez installer React DevTools:
   ```bash
   npm install -g react-devtools
   ```

### Dépannage courant

- **Erreur "Unable to resolve module"**: Redémarrez le bundler Metro avec l'option de reset du cache:
  ```bash
  npx expo start --clear
  ```

- **Problème de connexion à Expo Go**: Assurez-vous que votre téléphone et votre ordinateur sont sur le même réseau Wi-Fi

- **Expo Go ne peut pas ouvrir le projet**: Vérifiez si votre pare-feu ou votre antivirus bloque les connexions

## Metro Bundler

Metro est le bundler JavaScript utilisé par React Native et Expo. Il est responsable de l'empaquetage de votre code source JavaScript et de ses dépendances en un bundle qui peut être exécuté par l'application mobile.

### Rôle de Metro dans Expo

Metro joue plusieurs rôles cruciaux dans le développement Expo:

1. **Empaquetage du code**: Combine tous vos fichiers JavaScript en un seul bundle
2. **Transformation du code**: Convertit le JSX et ES6+ en JavaScript compatible
3. **Hot Reloading**: Permet la mise à jour instantanée de l'application pendant le développement
4. **Optimisation**: Minimise et optimise le code pour la production
5. **Gestion des assets**: Inclut et traite les images et autres ressources

### Démarrage de Metro

Lorsque vous exécutez `npx expo start`, Metro démarre automatiquement:

```bash
npx expo start
```

Vous verrez le Metro Bundler s'exécuter dans votre terminal, avec des informations sur l'état de l'empaquetage et des options disponibles.

### Configuration de Metro

Vous pouvez personnaliser le comportement de Metro en créant un fichier `metro.config.js` à la racine de votre projet:

```javascript
// metro.config.js
const { getDefaultConfig } = require('expo/metro-config');

const config = getDefaultConfig(__dirname);

// Personnalisations:
// Exemple: Ajouter des extensions de fichiers supplémentaires
config.resolver.sourceExts.push('mjs', 'cjs');

// Exemple: Configurer les transformations
config.transformer.babelTransformerPath = require.resolve('custom-transformer');

module.exports = config;
```

### Options de démarrage de Metro

Metro propose plusieurs options utiles:

```bash
# Démarrer avec reset du cache
npx expo start --clear

# Démarrer en mode production
npx expo start --no-dev

# Démarrer sans cache (utile pour les problèmes persistants)
npx expo start --no-cache

# Démarrer avec un port personnalisé
npx expo start --port 19001

# Démarrer en mode offline
npx expo start --offline

# Démarrer pour development build
npx expo start --dev-client
```

### Structure du bundle Metro

Metro crée différents types de bundles:

1. **Bundle de développement**: Non minimisé, avec code source mappé pour faciliter le débogage
2. **Bundle de production**: Minimisé et optimisé pour les performances
3. **Bundle delta**: Petits bundles incrémentaux pour le hot reloading

### Résolution des problèmes courants

#### Cache corrompu

Si vous rencontrez des erreurs bizarres, essayez de réinitialiser le cache de Metro:

```bash
npx expo start --clear
```

#### Modules non trouvés

Si Metro ne trouve pas un module:

```bash
# Vérifiez que le module est installé
npm install --save le-module-manquant

# Redémarrez avec un cache propre
npx expo start --clear
```

#### Problèmes de transformations

Pour les erreurs de transformation comme "Unable to resolve module":

1. Vérifiez les chemins d'importation
2. Assurez-vous que les extensions de fichier sont prises en charge
3. Redémarrez Metro avec l'option de reset du cache

#### Performances lentes

Si Metro est lent:

1. Limitez le nombre de transformations en réduisant la taille de votre code
2. Évitez les importations excessives ou circulaires
3. Utilisez des alias d'importation pour simplifier les chemins

### Hot Reloading vs Live Reloading

Metro prend en charge deux modes de rafraîchissement:

1. **Hot Reloading**: Met à jour uniquement les composants modifiés tout en préservant l'état
2. **Live Reloading**: Recharge complètement l'application après chaque modification

Vous pouvez basculer entre ces modes dans le menu de développement de l'application en appuyant sur:
- `Cmd+D` (iOS Simulator)
- `Ctrl+M` (Android Emulator)
- Secouez votre appareil physique

### Utilisation de Metro avec Expo EAS

Lors de la construction de votre application avec EAS Build, Metro est également utilisé pour créer le bundle de production:

```bash
# Exemple de build EAS production
npx eas build --platform ios --profile production
```

Dans ce cas, Metro s'exécute avec des optimisations de production comme:
- Minimisation du code
- Élimination du code mort
- Bundle optimisé pour la taille et les performances

## Expo Go vs Development Build

Expo offre deux approches principales pour le développement d'applications: **Expo Go** et les **Development Builds**. Comprendre leurs différences est essentiel pour choisir la méthode qui convient à votre projet.

### Expo Go

Expo Go est une application disponible sur l'App Store et le Google Play Store qui vous permet de tester rapidement vos projets Expo sans avoir à créer de builds natifs.

#### Avantages d'Expo Go

- **Démarrage rapide**: Commencez à développer en quelques minutes sans configuration complexe
- **Partage facile**: Partagez votre application via un lien QR avec n'importe qui ayant Expo Go installé
- **Pas de build natif**: Évitez d'attendre que les builds natifs se terminent pendant le développement
- **Mise à jour instantanée**: Les changements sont reflétés immédiatement sur votre appareil

#### Limitations d'Expo Go

- **Modules natifs limités**: Vous ne pouvez utiliser que les modules natifs déjà inclus dans Expo Go
- **Personnalisation restreinte**: Certaines configurations natives ne sont pas accessibles
- **Non destiné à la production**: Expo Go est uniquement un outil de développement

#### Quand utiliser Expo Go

- Pour les prototypes et les tests rapides
- Pour les nouveaux développeurs qui débutent avec React Native
- Pour des projets qui n'ont pas besoin de modules natifs personnalisés
- Pour tester des idées d'applications rapidement

### Development Builds

Les Development Builds sont des versions de votre application qui incluent les outils de développement d'Expo. Contrairement à Expo Go, elles sont compilées spécifiquement pour votre projet.

#### Avantages des Development Builds

- **Support des modules natifs personnalisés**: Utilisez n'importe quel module natif React Native
- **Contrôle total**: Accédez à toutes les configurations natives
- **Identique à la production**: L'environnement de développement est plus proche de la version finale
- **Extensions natives**: Ajoutez vos propres extensions natives si nécessaire

#### Création d'une Development Build

1. **Configurez EAS Build**:
   ```bash
   npx eas build:configure
   ```

2. **Créez un profil de développement** dans `eas.json`:
   ```json
   {
     "build": {
       "development": {
         "developmentClient": true,
         "distribution": "internal"
       }
     }
   }
   ```

3. **Construisez votre Development Build**:
   ```bash
   # Pour Android
   npx eas build --profile development --platform android
   
   # Pour iOS
   npx eas build --profile development --platform ios
   ```

4. **Installez le build** sur votre appareil une fois terminé

5. **Démarrez le serveur de développement**:
   ```bash
   npx expo start --dev-client
   ```

#### Quand utiliser les Development Builds

- Pour des projets qui nécessitent des modules natifs personnalisés
- Pour des applications destinées à la production
- Pour tester des fonctionnalités natives qui ne sont pas disponibles dans Expo Go
- Pour les environnements d'équipe où la cohérence entre développement et production est importante

### Tableau comparatif

| Fonctionnalité | Expo Go | Development Build |
|----------------|---------|-------------------|
| Facilité d'utilisation | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| Temps de mise en place | Minutes | Heures |
| Modules natifs personnalisés | ❌ | ✅ |
| Mise à jour instantanée | ✅ | ✅ |
| Proche de la production | ❌ | ✅ |
| Partage facile | ✅ | Limité |
| Outils de développement | ✅ | ✅ |

### Transition d'Expo Go vers Development Build

Lorsque votre projet évolue et nécessite des fonctionnalités non disponibles dans Expo Go, vous pouvez migrer vers une Development Build:

1. **Identifiez les modules natifs** dont vous avez besoin

2. **Installez les packages nécessaires**:
   ```bash
   npx expo install [nom-du-package]
   ```

3. **Configurez EAS Build** comme décrit précédemment

4. **Créez et utilisez votre Development Build**

Cette transition est transparente car les deux approches utilisent le même code base et les mêmes outils de développement, seule la méthode d'exécution change.

## Structure d'un projet Expo

La structure d'un projet Expo moderne peut varier selon que vous utilisez Expo Router ou non. Nous allons examiner les deux cas, en commençant par un projet Expo standard.

### Structure d'un projet Expo standard

```
mon-app/
├── assets/              # Images, polices et autres fichiers statiques
├── src/                 # Code source (organisation recommandée)
│   ├── components/      # Composants réutilisables
│   ├── screens/         # Écrans de l'application
│   ├── navigation/      # Configuration de la navigation (si React Navigation)
│   ├── hooks/           # Hooks personnalisés
│   ├── utils/           # Fonctions utilitaires
│   └── api/             # Services et appels API
├── node_modules/        # Dépendances du projet (géré par npm/yarn)
├── .gitignore           # Fichier de configuration Git
├── app.json             # Configuration de l'application Expo
├── app.config.js        # Configuration dynamique (remplace ou complète app.json)
├── App.js               # Point d'entrée de l'application
├── babel.config.js      # Configuration de Babel
├── package.json         # Dépendances et scripts
├── tsconfig.json        # Configuration TypeScript (si applicable)
└── README.md            # Documentation du projet
```

### Structure d'un projet avec Expo Router

Avec Expo Router, la structure du dossier `app` est particulièrement importante car elle définit les routes de votre application:

```
mon-app/
├── app/                 # Dossier principal pour Expo Router
│   ├── _layout.js       # Layout principal de l'application
│   ├── index.js         # Page d'accueil (route '/')
│   ├── about.js         # Page "à propos" (route '/about')
│   ├── (auth)/          # Groupe de routes d'authentification (n'affecte pas l'URL)
│   │   ├── login.js     # Écran de connexion (route '/login')
│   │   └── signup.js    # Écran d'inscription (route '/signup')
│   ├── profile/         # Dossier pour les routes imbriquées
│   │   ├── _layout.js   # Layout pour les routes de profil
│   │   ├── index.js     # Page de profil principale (route '/profile')
│   │   └── edit.js      # Page d'édition de profil (route '/profile/edit')
│   └── products/        # Dossier pour les produits
│       ├── index.js     # Liste des produits (route '/products')
│       └── [id].js      # Détail d'un produit (route dynamique '/products/123')
├── src/                 # Autres fichiers sources non liés aux routes
│   ├── components/      # Composants réutilisables
│   ├── hooks/           # Hooks personnalisés
│   ├── utils/           # Fonctions utilitaires
│   └── api/             # Services et appels API
├── assets/              # Images, polices et autres fichiers statiques
├── .gitignore           # Fichier de configuration Git
├── app.json             # Configuration de l'application Expo
├── package.json         # Dépendances et scripts
└── README.md            # Documentation du projet
```

### Le fichier app.json et app.config.js

Le fichier `app.json` (ou `app.config.js` pour une configuration dynamique) contient la configuration de votre application Expo:

```json
{
  "expo": {
    "name": "Mon Application",
    "slug": "mon-application",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "userInterfaceStyle": "light",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "assetBundlePatterns": ["**/*"],
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "com.company.myapp"
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      },
      "package": "com.company.myapp"
    },
    "web": {
      "favicon": "./assets/favicon.png"
    },
    "plugins": [
      "expo-router"
    ],
    "scheme": "myapp",
    "experiments": {
      "typedRoutes": true
    }
  }
}
```

Pour une configuration dynamique, vous pouvez utiliser `app.config.js`:

```javascript
export default {
  name: "Mon Application",
  slug: "mon-application",
  version: process.env.VERSION || "1.0.0",
  // ... reste de la configuration
  extra: {
    apiUrl: process.env.API_URL || "https://api.example.com",
  },
};
```

### Organisation recommandée des fichiers

Pour un projet Expo d'envergure, la structure recommandée est:

```
mon-app/
├── app/                       # Routes avec Expo Router
├── src/
│   ├── components/
│   │   ├── ui/                # Composants d'interface réutilisables 
│   │   │   ├── Button.jsx
│   │   │   ├── Card.jsx
│   │   │   └── index.js       # Export de tous les composants UI
│   │   └── features/          # Composants spécifiques aux fonctionnalités
│   │       ├── auth/
│   │       └── products/
│   ├── hooks/                 # Hooks personnalisés
│   │   ├── useAuth.js
│   │   └── useApi.js
│   ├── services/              # Services (auth, API, etc.)
│   │   ├── api.js
│   │   └── storage.js
│   ├── utils/                 # Utilitaires
│   │   ├── formatting.js
│   │   └── validation.js
│   ├── contexts/              # Contextes React
│   │   ├── AuthContext.js
│   │   └── ThemeContext.js
│   └── constants/             # Constantes
│       ├── colors.js
│       └── config.js
└── assets/
    ├── images/
    └── fonts/
```

### Gestion des assets

Expo simplifie la gestion des ressources comme les images, les polices et les fichiers audio:

#### Images

```javascript
import { Image } from 'react-native';

// Image locale
<Image source={require('../assets/images/logo.png')} />

// Image distante
<Image source={{ uri: 'https://exemple.com/image.jpg' }} />
```

#### Polices personnalisées

Pour utiliser des polices personnalisées, vous avez deux options:

1. **Avec expo-font**:

```javascript
import { useFonts } from 'expo-font';

export default function App() {
  const [fontsLoaded] = useFonts({
    'Montserrat': require('./assets/fonts/Montserrat-Regular.ttf'),
    'Montserrat-Bold': require('./assets/fonts/Montserrat-Bold.ttf'),
  });

  if (!fontsLoaded) {
    return null; // Ou un écran de chargement
  }

  return (
    // Votre application
  );
}
```

2. **Avec le module Expo Google Fonts**:

```bash
npx expo install @expo-google-fonts/inter expo-font
```

```javascript
import { useFonts, Inter_400Regular, Inter_700Bold } from '@expo-google-fonts/inter';

export default function App() {
  const [fontsLoaded] = useFonts({
    Inter_400Regular,
    Inter_700Bold,
  });

  if (!fontsLoaded) {
    return null;
  }

  return (
    // Votre application
  );
}
```

### Configuration TypeScript

Expo prend en charge TypeScript nativement. Pour un nouveau projet avec TypeScript:

```bash
npx create-expo-app@latest mon-app --template blank-typescript
```

La structure d'un projet TypeScript est similaire, mais avec des fichiers `.tsx` au lieu de `.jsx`.

## Composants fondamentaux de React Native

React Native fournit un ensemble de composants de base qui sont mappés aux éléments natifs sur iOS et Android.

### Composants essentiels

#### View

Équivalent à une `<div>` en web, `View` est un conteneur qui prend en charge les styles flexbox.

```javascript
import { View, StyleSheet } from 'react-native';

export default function MonComposant() {
  return (
    <View style={styles.container}>
      {/* Contenu */}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

#### Text

Composant pour afficher du texte.

```javascript
import { Text, StyleSheet } from 'react-native';

export default function MonComposant() {
  return (
    <Text style={styles.text}>Bonjour, Expo!</Text>
  );
}

const styles = StyleSheet.create({
  text: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
  },
});
```

#### Image

Composant pour afficher des images.

```javascript
import { Image, StyleSheet } from 'react-native';

export default function MonComposant() {
  return (
    <Image
      source={require('./assets/logo.png')}
      style={styles.image}
      resizeMode="contain"
    />
  );
}

const styles = StyleSheet.create({
  image: {
    width: 200,
    height: 200,
  },
});
```

#### ScrollView

Conteneur scrollable.

```javascript
import { ScrollView, Text, StyleSheet } from 'react-native';

export default function MonComposant() {
  return (
    <ScrollView style={styles.container}>
      {Array(20).fill().map((_, i) => (
        <Text key={i} style={styles.item}>Item {i+1}</Text>
      ))}
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  item: {
    padding: 20,
    borderBottomWidth: 1,
    borderBottomColor: '#ccc',
  },
});
```

#### FlatList

Liste optimisée pour de grandes quantités de données.

```javascript
import { FlatList, Text, StyleSheet } from 'react-native';

export default function MonComposant() {
  const donnees = Array(100).fill().map((_, i) => ({ id: i.toString(), titre: `Item ${i+1}` }));
  
  return (
    <FlatList
      data={donnees}
      renderItem={({ item }) => <Text style={styles.item}>{item.titre}</Text>}
      keyExtractor={item => item.id}
    />
  );
}

const styles = StyleSheet.create({
  item: {
    padding: 20,
    borderBottomWidth: 1,
    borderBottomColor: '#ccc',
  },
});
```

#### TextInput

Champ de saisie de texte.

```javascript
import { useState } from 'react';
import { TextInput, StyleSheet } from 'react-native';

export default function MonComposant() {
  const [texte, setTexte] = useState('');
  
  return (
    <TextInput
      style={styles.input}
      value={texte}
      onChangeText={setTexte}
      placeholder="Entrez votre texte"
    />
  );
}

const styles = StyleSheet.create({
  input: {
    height: 40,
    borderWidth: 1,
    borderColor: '#ccc',
    padding: 10,
    borderRadius: 5,
    width: '100%',
  },
});
```

#### Button et Touchable

Pour les interactions utilisateur:

```javascript
import { Button, TouchableOpacity, Text, StyleSheet } from 'react-native';

export default function MonComposant() {
  return (
    <>
      {/* Bouton standard */}
      <Button
        title="Appuyez-moi"
        onPress={() => alert('Bouton pressé!')}
        color="#841584"
      />
      
      {/* Bouton personnalisé */}
      <TouchableOpacity 
        style={styles.bouton}
        onPress={() => alert('TouchableOpacity pressé!')}
      >
        <Text style={styles.texteBouton}>Bouton personnalisé</Text>
      </TouchableOpacity>
    </>
  );
}

const styles = StyleSheet.create({
  bouton: {
    backgroundColor: '#3498db',
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
  },
  texteBouton: {
    color: 'white',
    fontWeight: 'bold',
  },
});
```

### Cycle de vie des composants React

Les composants React suivent un cycle de vie spécifique. Avec les hooks React:

```javascript
import React, { useState, useEffect } from 'react';
import { View, Text } from 'react-native';

export default function MonComposant() {
  const [compteur, setCompteur] = useState(0);
  
  // S'exécute après le premier rendu (montage)
  useEffect(() => {
    console.log('Composant monté');
    
    // Fonction de nettoyage (équivalent à componentWillUnmount)
    return () => {
      console.log('Composant démonté');
    };
  }, []); // Le tableau vide signifie: exécuter seulement au montage et démontage
  
  // S'exécute à chaque fois que compteur change
  useEffect(() => {
    console.log('Compteur mis à jour:', compteur);
  }, [compteur]);
  
  return (
    <View>
      <Text>Compteur: {compteur}</Text>
      <Button 
        title="Incrémenter" 
        onPress={() => setCompteur(compteur + 1)}
      />
    </View>
  );
}
```

## SafeArea et considérations de mise en page

La gestion des zones sécurisées (SafeArea) est cruciale dans le développement d'applications mobiles modernes, en particulier avec l'apparition des appareils à encoches, à coins arrondis et avec des gestes de navigation système.

### Comprendre la SafeArea

La SafeArea représente la zone de l'écran qui est garantie d'être visible et accessible à l'utilisateur, sans être obstruée par:

- Les encoches (notch) sur iPhone X et modèles ultérieurs
- Les barres de navigation système
- Les indicateurs de geste de retour à l'accueil
- Les coins arrondis des écrans

### SafeAreaView et SafeAreaContext

Pour gérer correctement ces zones, Expo fournit le module `react-native-safe-area-context`:

```bash
npx expo install react-native-safe-area-context
```

Ce module est déjà inclus par défaut dans les projets Expo Router.

#### Utilisation de base de SafeAreaView

```jsx
import { SafeAreaView } from 'react-native-safe-area-context';
import { Text, StyleSheet } from 'react-native';

export default function SafeScreen() {
  return (
    <SafeAreaView style={styles.container}>
      <Text>Ce contenu est dans la zone sécurisée</Text>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
});
```

#### Utilisation du hook useSafeAreaInsets

Pour un contrôle plus précis, vous pouvez utiliser le hook `useSafeAreaInsets`:

```jsx
import { View, Text, StyleSheet } from 'react-native';
import { useSafeAreaInsets } from 'react-native-safe-area-context';

export default function CustomSafeScreen() {
  const insets = useSafeAreaInsets();
  
  return (
    <View style={[
      styles.container,
      {
        // Appliquer les insets comme padding
        paddingTop: insets.top,
        paddingBottom: insets.bottom,
        paddingLeft: insets.left,
        paddingRight: insets.right,
      }
    ]}>
      <Text>Contenu avec insets personnalisés</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
});
```

### Configuration pour toute l'application

Pour appliquer la SafeArea à toute votre application, vous devez fournir le `SafeAreaProvider` à la racine:

```jsx
// Pour Expo Router, dans app/_layout.js
import { SafeAreaProvider } from 'react-native-safe-area-context';
import { Stack } from 'expo-router';

export default function RootLayout() {
  return (
    <SafeAreaProvider>
      <Stack />
    </SafeAreaProvider>
  );
}
```

### SafeArea et thème sombre/clair

Il est important d'adapter la SafeArea en fonction du thème de l'application:

```jsx
import { useColorScheme } from 'react-native';
import { SafeAreaView } from 'react-native-safe-area-context';

export default function ThemedSafeScreen() {
  const colorScheme = useColorScheme();
  const backgroundColor = colorScheme === 'dark' ? '#000' : '#fff';
  const textColor = colorScheme === 'dark' ? '#fff' : '#000';
  
  return (
    <SafeAreaView style={{ flex: 1, backgroundColor }}>
      <Text style={{ color: textColor }}>
        Contenu adapté au thème
      </Text>
    </SafeAreaView>
  );
}
```

### Ignorer certaines zones de SafeArea

Dans certains cas, vous pourriez vouloir ignorer certaines zones sécurisées:

```jsx
import { SafeAreaView } from 'react-native-safe-area-context';

export default function CustomSafeScreen() {
  return (
    <SafeAreaView 
      style={{ flex: 1, backgroundColor: '#fff' }}
      edges={['right', 'left', 'top']} // Ignore le bas
    >
      <Text>Cette vue ignore la zone sécurisée du bas</Text>
    </SafeAreaView>
  );
}
```

### Considérations pour les en-têtes et pieds de page

Pour les interfaces avec des barres fixes:

```jsx
import { View, Text, StyleSheet } from 'react-native';
import { useSafeAreaInsets } from 'react-native-safe-area-context';

export default function AppWithHeader() {
  const insets = useSafeAreaInsets();
  
  return (
    <View style={styles.container}>
      <View style={[
        styles.header,
        { paddingTop: insets.top } // Ajouter l'inset supérieur à l'en-tête
      ]}>
        <Text style={styles.headerText}>Mon application</Text>
      </View>
      
      <View style={styles.content}>
        <Text>Contenu principal</Text>
      </View>
      
      <View style={[
        styles.footer,
        { paddingBottom: insets.bottom } // Ajouter l'inset inférieur au pied de page
      ]}>
        <Text>Barre de navigation</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  header: {
    backgroundColor: '#3498db',
    paddingVertical: 10,
  },
  headerText: {
    color: 'white',
    textAlign: 'center',
    fontWeight: 'bold',
  },
  content: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  footer: {
    backgroundColor: '#3498db',
    paddingVertical: 10,
  },
});
```

### Gestion des orientations

La SafeArea doit s'adapter aux changements d'orientation:

```jsx
import { useState, useEffect } from 'react';
import { Dimensions } from 'react-native';
import { SafeAreaView } from 'react-native-safe-area-context';

export default function OrientationAwareScreen() {
  const [orientation, setOrientation] = useState(
    Dimensions.get('window').width > Dimensions.get('window').height 
      ? 'landscape' 
      : 'portrait'
  );
  
  useEffect(() => {
    const updateOrientation = () => {
      const { width, height } = Dimensions.get('window');
      setOrientation(width > height ? 'landscape' : 'portrait');
    };
    
    // Écouteur pour les changements de dimensions
    const subscription = Dimensions.addEventListener('change', updateOrientation);
    
    return () => {
      subscription.remove();
    };
  }, []);
  
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <Text>Orientation actuelle: {orientation}</Text>
    </SafeAreaView>
  );
}
```

### SafeArea et statusbar

Gestion coordonnée de la SafeArea et de la barre d'état:

```jsx
import { StatusBar } from 'expo-status-bar';
import { SafeAreaView } from 'react-native-safe-area-context';

export default function StatusBarSafeScreen() {
  const isDarkMode = useColorScheme() === 'dark';
  
  return (
    <SafeAreaView style={{ 
      flex: 1, 
      backgroundColor: isDarkMode ? '#000' : '#fff'
    }}>
      <StatusBar style={isDarkMode ? 'light' : 'dark'} />
      <Text>Contenu avec StatusBar adaptée</Text>
    </SafeAreaView>
  );
}
```

### Bonnes pratiques pour la SafeArea

1. **Toujours utiliser SafeAreaView** pour les conteneurs principaux
2. **Adapter les couleurs de fond** aux zones hors SafeArea
3. **Tester sur différents appareils** avec différentes configurations d'encoche
4. **Considérer les deux orientations** portrait et paysage
5. **Coordonner avec la StatusBar** pour une expérience visuelle cohérente

## Expo Router

Expo Router est le système de navigation moderne recommandé pour les applications Expo. Il utilise une approche basée sur le système de fichiers, similaire à Next.js, où la structure des dossiers et fichiers définit automatiquement les routes de l'application.

### Installation d'Expo Router

Pour créer un nouveau projet avec Expo Router:

```bash
# Créer un nouveau projet avec le template Expo Router
npx create-expo-app mon-app --template expo-router

# Ou pour ajouter Expo Router à un projet existant
npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar
```

Puis, configurez le point d'entrée dans `package.json`:

```json
{
  "main": "expo-router/entry"
}
```

Et ajoutez les plugins nécessaires dans `app.json`:

```json
{
  "expo": {
    "plugins": [
      "expo-router"
    ],
    "scheme": "myapp"
  }
}
```

### Structure de base d'un projet Expo Router

Avec Expo Router, la navigation est définie par la structure de dossiers et de fichiers dans le répertoire `app`:

```
app/
├── _layout.js         # Layout racine de l'application
├── index.js           # Page d'accueil (route '/')
├── about.js           # Page "à propos" (route '/about')
├── profile/
│   ├── _layout.js     # Layout pour la section profile
│   ├── index.js       # Page de profil principale (route '/profile')
│   └── settings.js    # Page de paramètres (route '/profile/settings')
└── details/
    ├── [id].js        # Route dynamique (route '/details/123', '/details/456', etc.)
    └── index.js       # Liste des détails (route '/details')
```

### Routage basé sur les fichiers

Le nom et l'emplacement des fichiers déterminent les routes:

1. **Routes statiques**: Les fichiers comme `about.js` ou `settings.js` créent des routes statiques.
2. **Routes imbriquées**: Les dossiers comme `profile/` créent des routes imbriquées.
3. **Routes d'index**: Les fichiers `index.js` représentent la route par défaut d'un dossier.
4. **Layouts**: Les fichiers `_layout.js` définissent des mises en page partagées pour un groupe de routes.

Exemple simplifié:

```jsx
// app/index.js - Route '/'
import { View, Text } from 'react-native';

export default function Home() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Page d'accueil</Text>
    </View>
  );
}

// app/about.js - Route '/about'
import { View, Text } from 'react-native';

export default function About() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>À propos</Text>
    </View>
  );
}
```

### Layouts partagés

Les fichiers `_layout.js` définissent la mise en page pour un groupe de routes:

```jsx
// app/_layout.js - Layout racine
import { Stack } from 'expo-router';

export default function RootLayout() {
  return (
    <Stack>
      <Stack.Screen name="index" options={{ title: 'Accueil' }} />
      <Stack.Screen name="about" options={{ title: 'À propos' }} />
      <Stack.Screen name="profile" options={{ headerShown: false }} />
    </Stack>
  );
}

// app/profile/_layout.js - Layout de la section profil
import { Tabs } from 'expo-router';

export default function ProfileLayout() {
  return (
    <Tabs>
      <Tabs.Screen name="index" options={{ title: 'Mon Profil' }} />
      <Tabs.Screen name="settings" options={{ title: 'Paramètres' }} />
    </Tabs>
  );
}
```

### Navigation avec Expo Router

Pour naviguer entre les routes, utilisez les composants `Link` ou le hook `useRouter`:

```jsx
// Navigation avec Link
import { Link } from 'expo-router';
import { View, Text } from 'react-native';

export default function Home() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center', gap: 10 }}>
      <Text>Page d'accueil</Text>
      <Link href="/about">Aller à la page À propos</Link>
      <Link href="/profile">Voir mon profil</Link>
      <Link href="/details/123">Voir le détail #123</Link>
    </View>
  );
}

// Navigation avec useRouter
import { useRouter } from 'expo-router';
import { View, Text, Button } from 'react-native';

export default function About() {
  const router = useRouter();
  
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>À propos</Text>
      <Button 
        title="Retour à l'accueil" 
        onPress={() => router.push('/')}
      />
      <Button 
        title="Retour" 
        onPress={() => router.back()}
      />
    </View>
  );
}
```

### Routes dynamiques

Les routes dynamiques sont créées en plaçant le nom du paramètre entre crochets dans le nom du fichier, comme `[id].js`. Cela permet de créer des routes qui correspondent à plusieurs chemins basés sur un segment de l'URL.

```
app/
├── details/
│   ├── [id].js        # Route dynamique qui correspond à '/details/123', '/details/456', etc.
│   └── index.js       # Route '/details'
```

Exemple d'un fichier de route dynamique:

```jsx
// app/details/[id].js
import { useLocalSearchParams } from 'expo-router';
import { View, Text, StyleSheet } from 'react-native';

export default function DetailsScreen() {
  // Récupérer le paramètre 'id' de l'URL
  const { id } = useLocalSearchParams();

  return (
    <View style={styles.container}>
      <Text>Détails de l'élément {id}</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

### Navigation vers des routes dynamiques

Pour naviguer vers des routes dynamiques, vous pouvez utiliser deux approches:

1. **Lien direct avec valeur statique**:

```jsx
// Navigation vers une route dynamique avec une valeur statique
import { Link } from 'expo-router';

export default function HomeScreen() {
  return (
    <View style={styles.container}>
      <Text>Accueil</Text>
      <Link href="/details/1">Voir le premier élément</Link>
      <Link href="/details/2">Voir le deuxième élément</Link>
    </View>
  );
}
```

2. **Lien avec objet de paramètres**:

```jsx
// Navigation vers une route dynamique avec paramètres
import { Link } from 'expo-router';

export default function HomeScreen() {
  return (
    <View style={styles.container}>
      <Text>Accueil</Text>
      <Link
        href={{
          pathname: '/details/[id]',
          params: { id: '123' },
        }}>
        Voir l'élément 123
      </Link>
    </View>
  );
}
```

### Groupes de routes et organisation

Expo Router permet d'organiser les routes en groupes pour mieux structurer votre application:

1. **Groupes conventionnels**: Dossiers avec préfixe `(groupe)` qui n'affectent pas l'URL.

```
app/
├── (tabs)/
│   ├── _layout.js     # Layout pour les onglets
│   ├── home.js        # Route '/home'
│   └── settings.js    # Route '/settings'
```

2. **Groupes cachés**: Dossiers avec préfixe `_` qui servent à organiser le code sans affecter la structure des routes.

```
app/
├── _components/       # Composants partagés (n'affecte pas les routes)
│   ├── Button.js
│   └── Card.js
```

### Authentification et routes protégées

Expo Router permet de créer facilement des routes protégées avec des redirections conditionnelles:

```jsx
// app/_layout.js
import { Slot, useSegments, useRouter } from 'expo-router';
import { useEffect, useState } from 'react';
import { AuthProvider, useAuth } from '../context/auth';

// Hook personnalisé pour la vérification d'authentification
function useProtectedRoute() {
  const { isAuthenticated } = useAuth();
  const segments = useSegments();
  const router = useRouter();

  useEffect(() => {
    const inAuthGroup = segments[0] === '(auth)';
    
    // Si l'utilisateur n'est pas authentifié et n'est pas dans le groupe auth,
    // rediriger vers login
    if (!isAuthenticated && !inAuthGroup) {
      router.replace('/login');
    } else if (isAuthenticated && inAuthGroup) {
      // Si l'utilisateur est authentifié mais est dans le groupe auth,
      // rediriger vers home
      router.replace('/');
    }
  }, [isAuthenticated, segments]);
}

function RootLayoutNav() {
  useProtectedRoute();
  return <Slot />;
}

export default function RootLayout() {
  return (
    <AuthProvider>
      <RootLayoutNav />
    </AuthProvider>
  );
}
```

### Gestion des erreurs

Expo Router permet de gérer les erreurs avec des pages d'erreur personnalisées:

```jsx
// app/_layout.js
import { Stack } from 'expo-router';
import { ErrorBoundary } from 'expo-router/error';

function ErrorComponent({ error }) {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Une erreur est survenue :</Text>
      <Text>{error.message}</Text>
    </View>
  );
}

export default function RootLayout() {
  return (
    <ErrorBoundary catchErrors="boundary" onError={(error) => {
      // Logique pour enregistrer l'erreur
      console.error(error);
    }}>
      <Stack>
        <Stack.Screen name="index" options={{ title: 'Accueil' }} />
        <Stack.Screen name="about" options={{ title: 'À propos' }} />
      </Stack>
    </ErrorBoundary>
  );
}
```

### Layouts imbriqués et types de navigations

Expo Router prend en charge plusieurs types de navigations qui peuvent être imbriquées:

1. **Stack Navigation**:

```jsx
import { Stack } from 'expo-router';

export default function Layout() {
  return (
    <Stack>
      <Stack.Screen name="index" options={{ title: 'Accueil' }} />
    </Stack>
  );
}
```

2. **Tab Navigation**:

```jsx
import { Tabs } from 'expo-router';
import { Ionicons } from '@expo/vector-icons';

export default function TabsLayout() {
  return (
    <Tabs>
      <Tabs.Screen 
        name="home" 
        options={{
          title: 'Accueil',
          tabBarIcon: ({ color }) => (
            <Ionicons name="home" size={24} color={color} />
          ),
        }}
      />
      <Tabs.Screen 
        name="profile" 
        options={{
          title: 'Profil',
          tabBarIcon: ({ color }) => (
            <Ionicons name="person" size={24} color={color} />
          ),
        }}
      />
    </Tabs>
  );
}
```

3. **Drawer Navigation**:

```jsx
import { Drawer } from 'expo-router/drawer';
import { Ionicons } from '@expo/vector-icons';

export default function DrawerLayout() {
  return (
    <Drawer>
      <Drawer.Screen 
        name="home" 
        options={{
          title: 'Accueil',
          drawerIcon: ({ color }) => (
            <Ionicons name="home" size={24} color={color} />
          ),
        }}
      />
      <Drawer.Screen 
        name="settings" 
        options={{
          title: 'Paramètres',
          drawerIcon: ({ color }) => (
            <Ionicons name="settings" size={24} color={color} />
          ),
        }}
      />
    </Drawer>
  );
}
```

### Liens profonds (Deep Linking)

Expo Router facilite la configuration des liens profonds:

```json
// app.json
{
  "expo": {
    "scheme": "myapp",
    "plugins": ["expo-router"]
  }
}
```

Cela permet de lancer votre application via des liens comme `myapp://details/123`. Les redirections sont automatiquement gérées par Expo Router.

### Exemple complet d'une application avec Expo Router

Voici un exemple plus complet d'une structure d'application avec Expo Router comprenant des onglets, des routes dynamiques, et une gestion d'authentification basique:

```
app/
├── _layout.js                # Layout racine avec vérification d'auth
├── (auth)/
│   ├── _layout.js            # Layout pour auth
│   ├── login.js              # Écran de connexion
│   └── register.js           # Écran d'inscription
├── (app)/
│   ├── _layout.js            # Layout pour application principale (tabs)
│   ├── home/
│   │   ├── index.js          # Écran d'accueil
│   │   └── notifications.js  # Notifications
│   ├── profile/
│   │   ├── index.js          # Profil principal
│   │   └── edit.js           # Édition de profil
│   └── products/
│       ├── index.js          # Liste des produits
│       └── [id].js           # Détail d'un produit
├── _components/
│   ├── ProductCard.js        # Composant de carte produit
│   └── AuthForm.js           # Formulaire d'authentification
└── context/
    └── auth.js               # Contexte d'authentification
```

Avec cette approche, Expo Router fournit une manière intuitive et moderne de gérer la navigation dans votre application Expo, en tirant parti de conventions de nommage de fichiers et de structure de dossiers pour définir automatiquement vos routes.

## Gestion d'état

La gestion de l'état est cruciale pour les applications React Native. Voici plusieurs approches.

### État local avec useState

Pour des états simples au niveau d'un composant:

```javascript
import { useState } from 'react';
import { View, Text, Button } from 'react-native';

export default function CompteurScreen() {
  const [compteur, setCompteur] = useState(0);
  
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Compteur: {compteur}</Text>
      <Button title="Incrémenter" onPress={() => setCompteur(compteur + 1)} />
      <Button title="Décrémenter" onPress={() => setCompteur(compteur - 1)} />
      <Button title="Réinitialiser" onPress={() => setCompteur(0)} />
    </View>
  );
}
```

### Contexte React (Context API)

Pour partager l'état entre plusieurs composants:

```javascript
// ThemeContext.js
import { createContext, useState, useContext } from 'react';

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  return useContext(ThemeContext);
}
```

Utilisation:

```javascript
// App.js
import { ThemeProvider } from './ThemeContext';
import MainApp from './MainApp';

export default function App() {
  return (
    <ThemeProvider>
      <MainApp />
    </ThemeProvider>
  );
}

// MainApp.js
import { View, Text, Button, StyleSheet } from 'react-native';
import { useTheme } from './ThemeContext';

export default function MainApp() {
  const { theme, toggleTheme } = useTheme();
  
  const styles = StyleSheet.create({
    container: {
      flex: 1,
      backgroundColor: theme === 'light' ? '#fff' : '#333',
      alignItems: 'center',
      justifyContent: 'center',
    },
    text: {
      color: theme === 'light' ? '#000' : '#fff',
    },
  });
  
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Thème actuel: {theme}</Text>
      <Button title="Changer de thème" onPress={toggleTheme} />
    </View>
  );
}
```

### Redux

Pour des applications plus complexes, Redux offre une gestion d'état prévisible et centralisée:

Installation:
```bash
npm install redux react-redux @reduxjs/toolkit
```

Exemple de base:

```javascript
// store/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0,
  },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    reset: (state) => {
      state.value = 0;
    },
  },
});

export const { increment, decrement, reset } = counterSlice.actions;
export default counterSlice.reducer;

// store/index.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

// App.js
import { Provider } from 'react-redux';
import { store } from './store';
import MainApp from './MainApp';

export default function App() {
  return (
    <Provider store={store}>
      <MainApp />
    </Provider>
  );
}

// CompteurScreen.js
import { useSelector, useDispatch } from 'react-redux';
import { View, Text, Button } from 'react-native';
import { increment, decrement, reset } from './store/counterSlice';

export default function CompteurScreen() {
  const compteur = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();
  
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Compteur: {compteur}</Text>
      <Button title="Incrémenter" onPress={() => dispatch(increment())} />
      <Button title="Décrémenter" onPress={() => dispatch(decrement())} />
      <Button title="Réinitialiser" onPress={() => dispatch(reset())} />
    </View>
  );
}
```

## Utilisation des API Expo

Expo fournit de nombreuses API pour accéder aux fonctionnalités natives des appareils.

### Accès à la caméra

```bash
npx expo install expo-camera
```

```javascript
import { useState, useEffect, useRef } from 'react';
import { StyleSheet, Text, View, TouchableOpacity, Image } from 'react-native';
import { Camera } from 'expo-camera';

export default function CameraScreen() {
  const [hasPermission, setHasPermission] = useState(null);
  const [type, setType] = useState(Camera.Constants.Type.back);
  const [photo, setPhoto] = useState(null);
  const cameraRef = useRef(null);

  useEffect(() => {
    (async () => {
      const { status } = await Camera.requestCameraPermissionsAsync();
      setHasPermission(status === 'granted');
    })();
  }, []);

  if (hasPermission === null) {
    return <View />;
  }
  if (hasPermission === false) {
    return <Text>Pas d'accès à la caméra</Text>;
  }

  const takePicture = async () => {
    if (cameraRef.current) {
      const options = { quality: 0.5, base64: true };
      const data = await cameraRef.current.takePictureAsync(options);
      setPhoto(data.uri);
    }
  };

  return (
    <View style={styles.container}>
      {photo ? (
        <View style={styles.preview}>
          <Image source={{ uri: photo }} style={styles.previewImage} />
          <TouchableOpacity
            style={styles.button}
            onPress={() => setPhoto(null)}>
            <Text style={styles.text}>Nouvelle photo</Text>
          </TouchableOpacity>
        </View>
      ) : (
        <Camera style={styles.camera} type={type} ref={cameraRef}>
          <View style={styles.buttonContainer}>
            <TouchableOpacity
              style={styles.flipButton}
              onPress={() => {
                setType(
                  type === Camera.Constants.Type.back
                    ? Camera.Constants.Type.front
                    : Camera.Constants.Type.back
                );
              }}>
              <Text style={styles.text}>Retourner</Text>
            </TouchableOpacity>
            <TouchableOpacity
              style={styles.captureButton}
              onPress={takePicture}>
              <View style={styles.captureCircle} />
            </TouchableOpacity>
          </View>
        </Camera>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  camera: {
    flex: 1,
  },
  buttonContainer: {
    flex: 1,
    backgroundColor: 'transparent',
    flexDirection: 'row',
    margin: 20,
    justifyContent: 'space-between',
    alignItems: 'flex-end',
  },
  flipButton: {
    alignSelf: 'flex-end',
    alignItems: 'center',
    backgroundColor: 'rgba(0,0,0,0.4)',
    borderRadius: 5,
    padding: 15,
  },
  captureButton: {
    alignSelf: 'center',
    alignItems: 'center',
    backgroundColor: 'rgba(255,255,255,0.3)',
    borderRadius: 50,
    height: 70,
    width: 70,
    justifyContent: 'center',
  },
  captureCircle: {
    borderWidth: 2,
    borderColor: 'white',
    height: 50,
    width: 50,
    borderRadius: 25,
    backgroundColor: 'white',
  },
  text: {
    fontSize: 18,
    color: 'white',
  },
  preview: {
    flex: 1,
    justifyContent: 'flex-end',
    alignItems: 'center',
  },
  previewImage: {
    width: '100%',
    height: '80%',
  },
  button: {
    backgroundColor: '#2196F3',
    padding: 20,
    borderRadius: 5,
    margin: 20,
  },
});
```

### Géolocalisation

```bash
npx expo install expo-location
```

```javascript
import { useState, useEffect } from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
import * as Location from 'expo-location';
import MapView, { Marker } from 'react-native-maps';

export default function LocationScreen() {
  const [location, setLocation] = useState(null);
  const [errorMsg, setErrorMsg] = useState(null);

  useEffect(() => {
    (async () => {
      let { status } = await Location.requestForegroundPermissionsAsync();
      if (status !== 'granted') {
        setErrorMsg('Permission refusée');
        return;
      }

      let location = await Location.getCurrentPositionAsync({});
      setLocation(location);
    })();
  }, []);

  const getLocation = async () => {
    let location = await Location.getCurrentPositionAsync({});
    setLocation(location);
  };

  return (
    <View style={styles.container}>
      {errorMsg ? (
        <Text style={styles.errorText}>{errorMsg}</Text>
      ) : location ? (
        <>
          <MapView
            style={styles.map}
            initialRegion={{
              latitude: location.coords.latitude,
              longitude: location.coords.longitude,
              latitudeDelta: 0.0922,
              longitudeDelta: 0.0421,
            }}
          >
            <Marker
              coordinate={{
                latitude: location.coords.latitude,
                longitude: location.coords.longitude,
              }}
              title="Ma position"
            />
          </MapView>
          <View style={styles.infoContainer}>
            <Text>Latitude: {location.coords.latitude}</Text>
            <Text>Longitude: {location.coords.longitude}</Text>
            <Text>Altitude: {location.coords.altitude}</Text>
            <Text>Précision: {location.coords.accuracy} mètres</Text>
            <Button title="Actualiser" onPress={getLocation} />
          </View>
        </>
      ) : (
        <Text>Chargement de la localisation...</Text>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  map: {
    width: '100%',
    height: '70%',
  },
  infoContainer: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  errorText: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
    color: 'red',
  },
});
```

### Stockage local

```bash
npx expo install expo-secure-store
```

```javascript
import { useState, useEffect } from 'react';
import { StyleSheet, Text, View, TextInput, Button } from 'react-native';
import * as SecureStore from 'expo-secure-store';

export default function StorageScreen() {
  const [key, setKey] = useState('myKey');
  const [value, setValue] = useState('');
  const [storedValue, setStoredValue] = useState('');

  useEffect(() => {
    getValueFor(key);
  }, [key]);

  async function save() {
    await SecureStore.setItemAsync(key, value);
    getValueFor(key);
  }

  async function getValueFor(key) {
    let result = await SecureStore.getItemAsync(key);
    if (result) {
      setStoredValue(result);
    } else {
      setStoredValue('Aucune valeur stockée');
    }
  }

  async function deleteValue() {
    await SecureStore.deleteItemAsync(key);
    getValueFor(key);
  }

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Stockage Sécurisé Expo</Text>
      
      <View style={styles.inputContainer}>
        <Text>Clé:</Text>
        <TextInput
          style={styles.input}
          value={key}
          onChangeText={setKey}
          placeholder="Entrez une clé"
        />
      </View>
      
      <View style={styles.inputContainer}>
        <Text>Valeur:</Text>
        <TextInput
          style={styles.input}
          value={value}
          onChangeText={setValue}
          placeholder="Entrez une valeur à stocker"
        />
      </View>
      
      <View style={styles.buttonContainer}>
        <Button title="Sauvegarder" onPress={save} />
        <Button title="Supprimer" onPress={deleteValue} color="red" />
      </View>
      
      <View style={styles.resultContainer}>
        <Text style={styles.subtitle}>Valeur stockée:</Text>
        <Text style={styles.storedValue}>{storedValue}</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
    textAlign: 'center',
  },
  subtitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  inputContainer: {
    marginBottom: 15,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    borderRadius: 5,
    padding: 10,
    marginTop: 5,
  },
  buttonContainer: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginVertical: 20,
  },
  resultContainer: {
    marginTop: 20,
    padding: 15,
    backgroundColor: '#f0f0f0',
    borderRadius: 5,
  },
  storedValue: {
    fontSize: 16,
    color: '#333',
  },
});
```

### Notifications push

```bash
npx expo install expo-notifications
```

```javascript
import { useState, useEffect, useRef } from 'react';
import { Text, View, Button, Platform } from 'react-native';
import * as Notifications from 'expo-notifications';
import * as Device from 'expo-device';

Notifications.setNotificationHandler({
  handleNotification: async () => ({
    shouldShowAlert: true,
    shouldPlaySound: true,
    shouldSetBadge: false,
  }),
});

export default function NotificationsScreen() {
  const [expoPushToken, setExpoPushToken] = useState('');
  const [notification, setNotification] = useState(false);
  const notificationListener = useRef();
  const responseListener = useRef();

  useEffect(() => {
    registerForPushNotificationsAsync().then(token => setExpoPushToken(token));

    notificationListener.current = Notifications.addNotificationReceivedListener(notification => {
      setNotification(notification);
    });

    responseListener.current = Notifications.addNotificationResponseReceivedListener(response => {
      console.log(response);
    });

    return () => {
      Notifications.removeNotificationSubscription(notificationListener.current);
      Notifications.removeNotificationSubscription(responseListener.current);
    };
  }, []);

  const schedulePushNotification = async () => {
    await Notifications.scheduleNotificationAsync({
      content: {
        title: "Notification locale 📬",
        body: 'Voici le contenu de votre notification locale',
        data: { data: 'donnees personnalisees' },
      },
      trigger: { seconds: 2 },
    });
  };

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Votre token Expo Push: {expoPushToken}</Text>
      <Text>Titre: {notification && notification.request.content.title} </Text>
      <Text>Corps: {notification && notification.request.content.body}</Text>
      <Text>Données: {notification && JSON.stringify(notification.request.content.data)}</Text>
      <Button
        title="Envoyer une notification dans 2 secondes"
        onPress={async () => {
          await schedulePushNotification();
        }}
      />
    </View>
  );
}

async function registerForPushNotificationsAsync() {
  let token;
  if (Device.isDevice) {
    const { status: existingStatus } = await Notifications.getPermissionsAsync();
    let finalStatus = existingStatus;
    if (existingStatus !== 'granted') {
      const { status } = await Notifications.requestPermissionsAsync();
      finalStatus = status;
    }
    if (finalStatus !== 'granted') {
      alert('Échec de l\'obtention du token pour les notifications push!');
      return;
    }
    token = (await Notifications.getExpoPushTokenAsync()).data;
    console.log(token);
  } else {
    alert('Les notifications push ne fonctionnent pas sur l\'émulateur');
  }

  if (Platform.OS === 'android') {
    Notifications.setNotificationChannelAsync('default', {
      name: 'default',
      importance: Notifications.AndroidImportance.MAX,
      vibrationPattern: [0, 250, 250, 250],
      lightColor: '#FF231F7C',
    });
  }

  return token;
}
```

## Styles et mise en page

### StyleSheet et styles de base

```javascript
import { StyleSheet, View, Text } from 'react-native';

export default function StylesExemple() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Titre principal</Text>
      <Text style={styles.subtitle}>Sous-titre</Text>
      <View style={styles.card}>
        <Text style={styles.cardText}>Contenu de la carte</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 10,
  },
  subtitle: {
    fontSize: 18,
    color: '#666',
    marginBottom: 20,
  },
  card: {
    backgroundColor: '#fff',
    borderRadius: 8,
    padding: 15,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3, // Pour Android
  },
  cardText: {
    fontSize: 16,
    color: '#333',
  },
});
```

### Flexbox

React Native utilise Flexbox pour la mise en page:

```javascript
import { StyleSheet, View, Text } from 'react-native';

export default function FlexboxExemple() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Mise en page avec Flexbox</Text>
      
      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Row - Space Between</Text>
        <View style={styles.rowSpaceBetween}>
          <View style={[styles.box, { backgroundColor: '#FF5252' }]} />
          <View style={[styles.box, { backgroundColor: '#FF9800' }]} />
          <View style={[styles.box, { backgroundColor: '#FFEB3B' }]} />
        </View>
      </View>
      
      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Row - Space Around</Text>
        <View style={styles.rowSpaceAround}>
          <View style={[styles.box, { backgroundColor: '#2196F3' }]} />
          <View style={[styles.box, { backgroundColor: '#4CAF50' }]} />
          <View style={[styles.box, { backgroundColor: '#9C27B0' }]} />
        </View>
      </View>
      
      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Column - Start</Text>
        <View style={styles.columnStart}>
          <View style={[styles.box, { backgroundColor: '#607D8B' }]} />
          <View style={[styles.box, { backgroundColor: '#795548' }]} />
          <View style={[styles.box, { backgroundColor: '#E91E63' }]} />
        </View>
      </View>
      
      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Flex Wrap</Text>
        <View style={styles.flexWrap}>
          {Array(8).fill().map((_, i) => (
            <View 
              key={i} 
              style={[
                styles.smallBox, 
                { backgroundColor: `hsl(${i * 45}, 70%, 60%)` }
              ]} 
            />
          ))}
        </View>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 22,
    fontWeight: 'bold',
    marginBottom: 20,
    textAlign: 'center',
  },
  section: {
    marginBottom: 20,
  },
  sectionTitle: {
    fontSize: 16,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  rowSpaceBetween: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    height: 50,
  },
  rowSpaceAround: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    height: 50,
  },
  columnStart: {
    flexDirection: 'column',
    alignItems: 'flex-start',
    height: 170,
    justifyContent: 'space-between',
  },
  flexWrap: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
  },
  box: {
    width: 50,
    height: 50,
    borderRadius: 4,
  },
  smallBox: {
    width: 70,
    height: 70,
    margin: 5,
    borderRadius: 4,
  },
});
```

### Responsive Design

```javascript
import { StyleSheet, View, Text, Dimensions, useWindowDimensions } from 'react-native';

// Obtenir les dimensions de l'écran
const screenWidth = Dimensions.get('window').width;
const screenHeight = Dimensions.get('window').height;

export default function ResponsiveDesignExemple() {
  // Hook pour récupérer les dimensions (meilleure approche car réagit aux changements d'orientation)
  const { width, height } = useWindowDimensions();
  
  const isLandscape = width > height;
  
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Design Responsive</Text>
      
      <Text>Largeur d'écran: {width}px</Text>
      <Text>Hauteur d'écran: {height}px</Text>
      <Text>Orientation: {isLandscape ? 'Paysage' : 'Portrait'}</Text>
      
      <View style={[
        styles.responsiveBox,
        {
          width: width > 600 ? '80%' : '95%',
          padding: width > 600 ? 30 : 15,
        }
      ]}>
        <Text style={[
          styles.responsiveText,
          { fontSize: width > 600 ? 20 : 16 }
        ]}>
          Cette boîte s'adapte à la taille de l'écran
        </Text>
      </View>
      
      <View style={isLandscape ? styles.rowLayout : styles.columnLayout}>
        <View style={[styles.card, isLandscape && styles.landscapeCard]}>
          <Text>Carte 1</Text>
        </View>
        <View style={[styles.card, isLandscape && styles.landscapeCard]}>
          <Text>Carte 2</Text>
        </View>
        <View style={[styles.card, isLandscape && styles.landscapeCard]}>
          <Text>Carte 3</Text>
        </View>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    alignItems: 'center',
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  responsiveBox: {
    backgroundColor: '#2196F3',
    borderRadius: 8,
    marginVertical: 20,
    alignItems: 'center',
  },
  responsiveText: {
    color: 'white',
    textAlign: 'center',
  },
  rowLayout: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    width: '100%',
  },
  columnLayout: {
    flexDirection: 'column',
    width: '100%',
  },
  card: {
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 8,
    marginVertical: 10,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  landscapeCard: {
    width: '30%',
  },
});
```

### Thèmes et styles globaux

Pour gérer les thèmes et styles globaux:

```javascript
// styles/theme.js
const colors = {
  primary: '#2196F3',
  secondary: '#FF9800',
  success: '#4CAF50',
  danger: '#F44336',
  warning: '#FFEB3B',
  info: '#00BCD4',
  light: '#F5F5F5',
  dark: '#212121',
  white: '#FFFFFF',
  black: '#000000',
  gray: '#9E9E9E',
  grayLight: '#E0E0E0',
  grayDark: '#616161',
};

const spacing = {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
  xxl: 48,
};

const fontSizes = {
  xs: 12,
  sm: 14,
  md: 16,
  lg: 18,
  xl: 20,
  xxl: 24,
  xxxl: 30,
};

const borderRadius = {
  xs: 2,
  sm: 4,
  md: 8,
  lg: 12,
  xl: 16,
  round: 1000,
};

const shadows = {
  light: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.18,
    shadowRadius: 1.0,
    elevation: 1,
  },
  medium: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 3,
  },
  dark: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.3,
    shadowRadius: 4.65,
    elevation: 6,
  },
};

const typography = {
  fontFamily: {
    regular: 'System',
    medium: 'System',
    bold: 'System',
  },
  h1: {
    fontSize: fontSizes.xxxl,
    fontWeight: 'bold',
    marginBottom: spacing.md,
  },
  h2: {
    fontSize: fontSizes.xxl,
    fontWeight: 'bold',
    marginBottom: spacing.md,
  },
  h3: {
    fontSize: fontSizes.xl,
    fontWeight: 'bold',
    marginBottom: spacing.sm,
  },
  body: {
    fontSize: fontSizes.md,
  },
  caption: {
    fontSize: fontSizes.xs,
    color: colors.gray,
  }
};

export default {
  colors,
  spacing,
  fontSizes,
  borderRadius,
  shadows,
  typography,
  // Styles communs réutilisables
  container: {
    flex: 1,
    padding: spacing.md,
    backgroundColor: colors.light,
  },
  card: {
    backgroundColor: colors.white,
    borderRadius: borderRadius.md,
    padding: spacing.md,
    ...shadows.medium,
    marginBottom: spacing.md,
  },
  button: {
    primary: {
      backgroundColor: colors.primary,
      paddingVertical: spacing.sm,
      paddingHorizontal: spacing.md,
      borderRadius: borderRadius.md,
      alignItems: 'center',
      justifyContent: 'center',
    },
    text: {
      color: colors.white,
      fontWeight: 'bold',
    }
  },
};
```

Utilisation du thème:

```javascript
// composants/Button.js
import { TouchableOpacity, Text, StyleSheet } from 'react-native';
import theme from '../styles/theme';

export default function Button({ title, onPress, variant = 'primary', size = 'medium', disabled }) {
  const buttonStyles = [styles.button];
  const textStyles = [styles.text];
  
  // Gérer les variantes
  if (variant === 'secondary') {
    buttonStyles.push(styles.secondaryButton);
    textStyles.push(styles.secondaryText);
  } else if (variant === 'outline') {
    buttonStyles.push(styles.outlineButton);
    textStyles.push(styles.outlineText);
  }
  
  // Gérer les tailles
  if (size === 'small') {
    buttonStyles.push(styles.smallButton);
    textStyles.push(styles.smallText);
  } else if (size === 'large') {
    buttonStyles.push(styles.largeButton);
    textStyles.push(styles.largeText);
  }
  
  // Gérer l'état désactivé
  if (disabled) {
    buttonStyles.push(styles.disabledButton);
    textStyles.push(styles.disabledText);
  }
  
  return (
    <TouchableOpacity 
      style={buttonStyles} 
      onPress={onPress}
      disabled={disabled}
    >
      <Text style={textStyles}>{title}</Text>
    </TouchableOpacity>
  );
}

const styles = StyleSheet.create({
  button: {
    ...theme.button.primary,
    marginVertical: theme.spacing.sm,
  },
  text: {
    ...theme.button.text,
    fontSize: theme.fontSizes.md,
  },
  secondaryButton: {
    backgroundColor: theme.colors.secondary,
  },
  outlineButton: {
    backgroundColor: 'transparent',
    borderWidth: 1,
    borderColor: theme.colors.primary,
  },
  outlineText: {
    color: theme.colors.primary,
  },
  smallButton: {
    paddingVertical: theme.spacing.xs,
    paddingHorizontal: theme.spacing.sm,
  },
  smallText: {
    fontSize: theme.fontSizes.sm,
  },
  largeButton: {
    paddingVertical: theme.spacing.md,
    paddingHorizontal: theme.spacing.lg,
  },
  largeText: {
    fontSize: theme.fontSizes.lg,
  },
  disabledButton: {
    backgroundColor: theme.colors.grayLight,
  },
  disabledText: {
    color: theme.colors.gray,
  },
});
```

## Tests et débogage

### Débogage avec Expo

Expo offre plusieurs outils de débogage:

1. **Menu de développement**: Accessible en secouant l'appareil ou via `cmd+D` (iOS) / `ctrl+M` (Android) sur émulateur

Options du menu développeur:
- Reload: Recharge l'application
- Debug Remote JS: Ouvre le débogueur JavaScript
- Toggle Element Inspector: Outil d'inspection des éléments
- Show Perf Monitor: Moniteur de performance

### Console.log et débogage dans Chrome

```javascript
import { useEffect } from 'react';
import { View, Text, Button } from 'react-native';

export default function DebugScreen() {
  useEffect(() => {
    console.log('DebugScreen monté');
    
    const obj = {
      a: 1,
      b: 'test',
      c: { nested: true },
    };
    
    console.log('Objet complexe:', obj);
    console.warn('Avertissement');
    console.error('Erreur');
    
    return () => {
      console.log('DebugScreen démonté');
    };
  }, []);
  
  const handlePress = () => {
    try {
      const result = someFunction(); // Cette fonction n'existe pas
      console.log(result);
    } catch (error) {
      console.error('Erreur capturée:', error);
    }
  };
  
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Écran de débogage</Text>
      <Button title="Provoquer une erreur" onPress={handlePress} />
    </View>
  );
}
```

Pour déboguer dans Chrome:
1. Ouvrez le menu développeur
2. Sélectionnez "Debug JS Remotely" ou "Debug"
3. Une fenêtre Chrome s'ouvrira
4. Ouvrez les DevTools (F12 ou Cmd+Opt+I)
5. Allez dans l'onglet "Sources" ou "Console"
6. Vous pouvez maintenant placer des points d'arrêt et suivre l'exécution

### Tests unitaires avec Jest

Configuration de Jest:

```bash
npm install --save-dev jest @testing-library/react-native @testing-library/jest-native
```

Fichier `jest.config.js`:

```javascript
module.exports = {
  preset: 'jest-expo',
  transformIgnorePatterns: [
    'node_modules/(?!(jest-)?react-native|@react-native|expo(nent)?|@expo(nent)?/.*|react-navigation|@react-navigation/.*|@unimodules/.*|unimodules|sentry-expo|native-base|expo-font)',
  ],
  setupFilesAfterEnv: ['@testing-library/jest-native/extend-expect'],
};
```

Exemple de test unitaire:

```javascript
// composants/Button.test.js
import React from 'react';
import { render, fireEvent } from '@testing-library/react-native';
import Button from './Button';

describe('Button Component', () => {
  it('renders correctly with title', () => {
    const { getByText } = render(<Button title="Press me" onPress={() => {}} />);
    expect(getByText('Press me')).toBeTruthy();
  });
  
  it('calls onPress when pressed', () => {
    const onPressMock = jest.fn();
    const { getByText } = render(<Button title="Press me" onPress={onPressMock} />);
    
    fireEvent.press(getByText('Press me'));
    expect(onPressMock).toHaveBeenCalledTimes(1);
  });
  
  it('applies disabled styles when disabled', () => {
    const { getByText } = render(
      <Button title="Press me" onPress={() => {}} disabled />
    );
    
    const buttonText = getByText('Press me');
    // Vérifiez que le style approprié est appliqué
    // Cela dépend de l'implémentation spécifique des styles dans votre composant
    expect(buttonText.props.style).toContainEqual(
      expect.objectContaining({ color: expect.any(String) })
    );
  });
});
```

### Tests d'intégration et End-to-End

Pour des tests plus complets, vous pouvez utiliser Detox ou Maestro:

#### Configuration de Maestro (plus récent et plus simple)

1. **Installation de Maestro**:
   ```bash
   # Pour macOS
   brew tap mobile-dev-inc/tap
   brew install maestro
   
   # Pour Windows/Linux, consultez la documentation officielle
   ```

2. **Création d'un test Maestro**:
   Créez un fichier `flow.yaml` dans le dossier `tests/`:
   ```yaml
   appId: com.yourcompany.yourapp
   ---
   - launchApp
   - takeScreenshot: "initial-screen"
   - tapOn:
       text: "Press me"
   - assertVisible:
       text: "Button pressed!"
   - takeScreenshot: "after-button-press"
   ```

3. **Exécution du test**:
   ```bash
   maestro test tests/flow.yaml
   ```

## Publication et déploiement

### Configuration pour la production

Modifiez `app.json` pour préparer votre application à la production:

```json
{
  "expo": {
    "name": "Mon Application",
    "slug": "mon-application",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "updates": {
      "fallbackToCacheTimeout": 0,
      "url": "https://u.expo.dev/YOUR_PROJECT_ID"
    },
    "assetBundlePatterns": [
      "**/*"
    ],
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "com.votrecompagnie.monapplication",
      "buildNumber": "1.0.0"
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#FFFFFF"
      },
      "package": "com.votrecompagnie.monapplication",
      "versionCode": 1,
      "permissions": [
        "CAMERA",
        "ACCESS_FINE_LOCATION"
      ]
    },
    "web": {
      "favicon": "./assets/favicon.png"
    },
    "extra": {
      "eas": {
        "projectId": "YOUR_PROJECT_ID"
      }
    }
  }
}
```

### Construction de l'application avec EAS Build

EAS (Expo Application Services) Build est le service officiel d'Expo pour construire des applications prêtes pour les stores.

Installation d'EAS CLI:

```bash
npm install -g eas-cli
```

Connexion à votre compte Expo:

```bash
eas login
```

Configuration d'EAS Build:

```bash
eas build:configure
```

Cela crée un fichier `eas.json`:

```json
{
  "cli": {
    "version": ">= 0.52.0"
  },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal"
    },
    "production": {}
  },
  "submit": {
    "production": {}
  }
}
```

Lancement d'un build:

```bash
# Pour Android
eas build --platform android --profile production

# Pour iOS
eas build --platform ios --profile production

# Pour les deux plateformes
eas build --platform all --profile production
```

### Mises à jour OTA (Over-The-Air) avec EAS Update

Les mises à jour OTA permettent de mettre à jour votre application sans passer par les stores.

Configuration dans `eas.json`:

```json
{
  "cli": {
    "version": ">= 0.52.0"
  },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal",
      "channel": "development"
    },
    "preview": {
      "distribution": "internal",
      "channel": "preview"
    },
    "production": {
      "channel": "production"
    }
  },
  "submit": {
    "production": {}
  }
}
```

Publier une mise à jour:

```bash
eas update --branch production --message "Correction de bugs"
```

Configuration côté code:

```javascript
// App.js
import { useEffect } from 'react';
import * as Updates from 'expo-updates';

export default function App() {
  useEffect(() => {
    checkForUpdates();
  }, []);

  async function checkForUpdates() {
    try {
      const update = await Updates.checkForUpdateAsync();
      
      if (update.isAvailable) {
        await Updates.fetchUpdateAsync();
        // Vous pouvez choisir d'appliquer immédiatement ou de demander à l'utilisateur
        await Updates.reloadAsync();
      }
    } catch (error) {
      console.error('Erreur lors de la vérification des mises à jour:', error);
    }
  }
  
  // Le reste de votre application
}
```

### Soumission aux stores avec EAS Submit

EAS Submit permet de soumettre votre application aux App Stores.

Configuration minimale:

```bash
# Configuration initiale
eas submit:configure
```

Soumettre l'application:

```bash
# Pour Android
eas submit --platform android

# Pour iOS
eas submit --platform ios
```

Vous aurez besoin:
- Pour Android: un compte développeur Google Play et un fichier de clés
- Pour iOS: un compte développeur Apple et des profils de provisionnement

## Expo EAS (Expo Application Services)

### EAS Build

EAS Build est un service de build dans le cloud qui:
- Construit vos applications natives (IPA, APK, AAB)
- Gère les dépendances
- Gère les credentials et signing
- Fonctionne avec des files d'attente de build

Options de configuration principales dans `eas.json`:

```json
{
  "build": {
    "production": {
      "node": "16.13.0",
      "env": {
        "API_URL": "https://api.production.com"
      },
      "android": {
        "buildType": "app-bundle",
        "gradleCommand": ":app:bundleRelease"
      },
      "ios": {
        "buildConfiguration": "Release",
        "credentialsSource": "remote"
      }
    },
    "preview": {
      "extends": "production",
      "env": {
        "API_URL": "https://api.staging.com"
      },
      "distribution": "internal"
    },
    "development": {
      "extends": "production",
      "developmentClient": true,
      "env": {
        "API_URL": "https://api.dev.com"
      },
      "distribution": "internal"
    }
  }
}
```

### EAS Update

EAS Update permet des mises à jour instantanées sans passer par les stores:

Principales fonctionnalités:
- Mises à jour du code JavaScript et des assets
- Système de branches (production, staging, etc.)
- Possibilité de rollback
- Déploiement progressif (pourcentage d'utilisateurs)

Configuration:

```javascript
// app.json
{
  "expo": {
    "updates": {
      "enabled": true,
      "fallbackToCacheTimeout": 0,
      "url": "https://u.expo.dev/your-project-id"
    },
    "runtimeVersion": {
      "policy": "sdkVersion"
    }
  }
}
```

Commandes principales:

```bash
# Publier une mise à jour
eas update --branch production --message "Nouvelle fonctionnalité"

# Publier sur une branche spécifique
eas update --branch staging --message "Correctif pour la version beta"

# Voir l'historique des mises à jour
eas update:list

# Rollback à une version précédente
eas update:rollback --branch production
```

### EAS Submit

EAS Submit automatise le processus de soumission aux stores:

Pour iOS:
```bash
eas submit --platform ios \
  --app-name "Mon Application" \
  --app-version "1.0.0" \
  --build-number "1"
```

Pour Android:
```bash
eas submit --platform android \
  --track production \
  --release-status completed
```

### Développement avec EAS

Le client de développement EAS permet de tester des modules natifs:

```bash
# Installer le client de développement
eas build:configure

# Construire le client de développement 
eas build --profile development --platform android
# ou
eas build --profile development --platform ios

# Démarrer le serveur de développement avec la prise en charge du client
npx expo start --dev-client
```

## Bonnes pratiques

### Architecture et organisation du code

1. **Structure des dossiers** (comme vue précédemment)
```
mon-projet-expo/
├── app/                 # Routes avec Expo Router
├── src/
│   ├── components/      # Composants réutilisables 
│   ├── hooks/           # Hooks personnalisés
│   ├── services/        # Services (API, stockage, etc.)
│   ├── utils/           # Fonctions utilitaires 
│   └── context/         # Contextes React
├── assets/              # Ressources statiques
└── ...
```

2. **Composants réutilisables**

Créez une bibliothèque de composants réutilisables:

```javascript
// src/components/ui/Button.js
export default function Button({ title, onPress, variant, ...props }) {
  // Implémentation
}

// src/components/ui/index.js (barrel file)
export { default as Button } from './Button';
export { default as Card } from './Card';
// etc.
```

3. **Custom Hooks**

```javascript
// src/hooks/useAPI.js
import { useState, useEffect } from 'react';

export default function useAPI(endpoint, options = {}) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(endpoint, options);
        const json = await response.json();
        setData(json);
        setError(null);
      } catch (err) {
        setError(err);
        setData(null);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [endpoint]);

  return { data, loading, error };
}
```

### Performance

1. **Memoization avec useMemo et useCallback**

```javascript
import { useMemo, useCallback } from 'react';

// Exemple avec useMemo pour éviter des calculs coûteux répétés
const expensiveValue = useMemo(() => {
  console.log('Calculating...');
  let result = 0;
  for (let i = 0; i < 1000000; i++) {
    result += i;
  }
  return result;
}, []);

// Exemple avec useCallback pour stabiliser les références de fonctions
const handlePress = useCallback(() => {
  console.log('Button pressed');
  // logique ici
}, []);
```

2. **Optimisation des listes avec FlatList**

```javascript
import { FlatList } from 'react-native';

// Utiliser getItemLayout pour optimiser le rendu
<FlatList
  data={items}
  renderItem={renderItem}
  keyExtractor={(item) => item.id}
  initialNumToRender={10}
  maxToRenderPerBatch={10}
  windowSize={5}
  getItemLayout={(data, index) => ({
    length: ITEM_HEIGHT,
    offset: ITEM_HEIGHT * index,
    index,
  })}
/>
```

### Sécurité

1. **Stockage sécurisé**

Utilisez `expo-secure-store` pour les données sensibles:

```javascript
import * as SecureStore from 'expo-secure-store';

// Stocker un token
await SecureStore.setItemAsync('auth_token', token);

// Récupérer le token
const token = await SecureStore.getItemAsync('auth_token');
```

2. **ENV variables**

Utilisez des variables d'environnement pour les clés API et secrets:

```javascript
// app.config.js
export default {
  expo: {
    // Autres configurations...
    extra: {
      apiKey: process.env.API_KEY,
      apiUrl: process.env.API_URL,
    },
  },
};

// Utilisation
import Constants from 'expo-constants';
const apiKey = Constants.manifest.extra.apiKey;
```

3. **Validation des entrées**

```javascript
function validateEmail(email) {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return re.test(email);
}

function handleSubmit() {
  if (!validateEmail(email)) {
    setError('Email invalide');
    return;
  }
  
  // Continue avec la soumission
}
```

### Accessibilité

1. **Labels accessibles**

```javascript
import { View, TouchableOpacity, Text } from 'react-native';

export default function AccessibleButton({ title, onPress }) {
  return (
    <TouchableOpacity
      onPress={onPress}
      accessible={true}
      accessibilityLabel={title}
      accessibilityRole="button"
      accessibilityHint={`Appuyez pour ${title.toLowerCase()}`}
    >
      <Text>{title}</Text>
    </TouchableOpacity>
  );
}
```

2. **Navigation au clavier**

```javascript
<TouchableOpacity
  accessible={true}
  accessibilityLabel="Appuyez pour continuer"
  accessibilityTraits="button"
  onAccessibilityTap={onPress} // Déclenché par les lecteurs d'écran
  onPress={onPress}
>
  <Text>Continuer</Text>
</TouchableOpacity>
```

3. **Contraste et taille de texte**

```javascript
const styles = StyleSheet.create({
  accessibleText: {
    fontSize: 16, // Minimum recommandé
    color: '#000', // Bon contraste sur fond blanc
    fontWeight: 'bold',
  },
});
```

## Ressources et liens utiles

### Documentation officielle

- [Documentation Expo](https://docs.expo.dev/)
- [Documentation React Native](https://reactnative.dev/docs/getting-started)
- [Documentation Expo Router](https://docs.expo.dev/router/introduction/)
- [Documentation EAS](https://docs.expo.dev/eas/)

### Bibliothèques recommandées

- **UI/UX**: 
  - [React Native Paper](https://callstack.github.io/react-native-paper/)
  - [NativeBase](https://nativebase.io/)
  - [React Native Elements](https://reactnativeelements.com/)
  
- **Gestion d'état**:
  - [Redux Toolkit](https://redux-toolkit.js.org/)
  - [Zustand](https://github.com/pmndrs/zustand)
  - [Jotai](https://jotai.org/)
  
- **Formulaires**:
  - [Formik](https://formik.org/)
  - [React Hook Form](https://react-hook-form.com/)
  
- **Animation**:
  - [React Native Reanimated](https://docs.swmansion.com/react-native-reanimated/)
  - [React Native Gesture Handler](https://docs.swmansion.com/react-native-gesture-handler/)
  
- **Internationalisation**:
  - [i18next](https://www.i18next.com/)
  - [react-native-localize](https://github.com/zoontek/react-native-localize)

### Communauté et support

- [Forum Expo](https://forums.expo.dev/)
- [GitHub Expo](https://github.com/expo/expo)
- [Stack Overflow - Expo](https://stackoverflow.com/questions/tagged/expo)
- [Discord React Native](https://discord.gg/reactiflux)

### Outils de développement

- [Expo Snack](https://snack.expo.dev/) - Playground en ligne pour Expo
- [React Native Debugger](https://github.com/jhen0409/react-native-debugger) - Outil de débogage avancé
- [Flipper](https://fbflipper.com/) - Plateforme de débogage pour iOS et Android

### Tutoriels et cours

- [Expo Blog](https://blog.expo.dev/) - Articles et tutoriels officiels
- [Expo YouTube](https://www.youtube.com/c/expo) - Vidéos tutoriels et démonstrations
- [React Native Express](https://www.reactnative.express/) - Guide interactif
- [Udemy](https://www.udemy.com/topic/react-native/) - Cours en ligne

---

Ce cours couvre l'ensemble des concepts fondamentaux d'Expo et React Native, vous donnant une base solide pour développer des applications mobiles. En utilisant les outils et techniques présentés, vous pourrez créer des applications performantes, maintenues et prêtes pour la production.