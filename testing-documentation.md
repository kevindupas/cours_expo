# Documentation des Tests pour Application React Native

## Table des matières

1. [Introduction aux tests](#introduction-aux-tests)
2. [Configuration de l'environnement de test](#configuration-de-lenvironnement-de-test)
3. [Structure des tests](#structure-des-tests)
4. [Types de tests implémentés](#types-de-tests-implémentés)
5. [Tests de composants](#tests-de-composants)
6. [Tests d'API](#tests-dapi)
7. [Exécution des tests](#exécution-des-tests)
8. [Bonnes pratiques](#bonnes-pratiques)
9. [Résolution des problèmes courants](#résolution-des-problèmes-courants)

## Introduction aux tests

Les tests sont une partie essentielle du développement logiciel qui garantit la qualité et la fiabilité de votre code. Dans ce projet, nous avons mis en place différents types de tests pour une application React Native :

- **Tests unitaires** : Testent des unités individuelles de code (comme les composants)
- **Tests d'intégration** : Testent l'interaction entre différentes parties du système (comme les appels API)

L'approche adoptée ici couvre les aspects critiques de l'application, notamment l'authentification et la récupération de données depuis l'API.

## Configuration de l'environnement de test

### Installation des dépendances

Pour configurer l'environnement de test dans votre projet React Native, exécutez les commandes suivantes :

```bash
# Installation de Jest et des bibliothèques de test associées
npm install --save-dev jest jest-expo @testing-library/react-native @testing-library/jest-native

# Pour les tests d'API
npm install --save-dev node-fetch@2 @types/node-fetch

# Types pour TypeScript
npm install --save-dev @types/jest @types/react-test-renderer
```

### Configuration de Jest

Jest est le framework de test utilisé dans ce projet. La configuration est définie dans le fichier `jest.config.js` :

```javascript
module.exports = {
  preset: 'jest-expo',
  setupFilesAfterEnv: [
    '@testing-library/jest-native/extend-expect'
  ],
  transformIgnorePatterns: [
    'node_modules/(?!((jest-)?react-native|@react-native(-community)?)|expo(nent)?|@expo(nent)?/.*|@expo-google-fonts/.*|react-navigation|@react-navigation/.*|@unimodules/.*|unimodules|sentry-expo|native-base|react-native-svg|expo-secure-store|twrnc|@sentry|expo-router))'
  ],
  
  // Alias pour la résolution des chemins
  moduleNameMapper: {
    '^@/contexts/(.*)$': '<rootDir>/contexts/$1',
    '^@/auth/(.*)$': '<rootDir>/app/auth/$1',
    '^@/hooks/(.*)$': '<rootDir>/hooks/$1',
    '^@/(.*)$': '<rootDir>/$1'
  },
  
  testMatch: [
    '**/__tests__/**/*.ts?(x)',
    '**/?(*.)+(spec|test).ts?(x)'
  ],
  
  moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node']
}
```

### Fichier de configuration Jest

Le fichier `jest.setup.js` contient la configuration globale pour les tests, notamment les mocks pour les dépendances externes :

```javascript
// jest.setup.js
import 'react-native-gesture-handler/jestSetup';

// Mock pour expo-router
const mockRouter = {
  replace: jest.fn(),
  push: jest.fn()
};

jest.mock('expo-router', () => {
  const segments = [''];
  segments.isReady = true;
  
  return {
    useRouter: () => mockRouter,
    useSegments: () => segments,
    Link: () => 'Link',
    Stack: {
      Screen: () => 'Stack.Screen'
    },
    Tabs: {
      Screen: () => 'Tabs.Screen'
    }
  };
});

// Mock pour expo-secure-store
jest.mock('expo-secure-store', () => {
  const mockStore = {};
  
  return {
    getItemAsync: jest.fn((key) => Promise.resolve(mockStore[key] || null)),
    setItemAsync: jest.fn((key, value) => {
      mockStore[key] = value;
      return Promise.resolve();
    }),
    deleteItemAsync: jest.fn((key) => {
      delete mockStore[key];
      return Promise.resolve();
    }),
    AFTER_FIRST_UNLOCK: 'AFTER_FIRST_UNLOCK'
  };
});

// Mock de fetch global
global.fetch = jest.fn().mockResolvedValue({
  ok: true,
  json: jest.fn().mockResolvedValue({})
});

// Désactive les avertissements console
jest.spyOn(console, 'warn').mockImplementation(() => {});
jest.spyOn(console, 'error').mockImplementation(() => {});
```

Pour activer ce fichier de configuration, décommentez la ligne correspondante dans `jest.config.js` :

```javascript
setupFiles: [
  './jest.setup.js'  // Important : référence le fichier de setup
],
```

## Structure des tests

Les tests sont organisés dans une structure de répertoires qui reflète la structure de l'application :

```
__test__/
  ├── api/                  # Tests d'API
  │   ├── auth.test.ts      # Test d'authentification
  │   ├── notes-list.test.ts # Test de récupération des notes
  │   └── note-by-id.test.ts # Test de récupération d'une note spécifique
  │
  ├── components/          # Tests de composants
  │   └── ...
  │
  └── contexts/            # Tests de contextes
      └── AuthContext.test.tsx
```

## Types de tests implémentés

### Tests de composants

Les tests de composants vérifient que les composants React Native fonctionnent correctement, en s'assurant qu'ils :
- Sont correctement rendus
- Réagissent correctement aux interactions utilisateur
- Affichent les bonnes données

### Tests d'API

Les tests d'API vérifient que les appels à l'API fonctionnent correctement, en s'assurant que :
- L'authentification fonctionne
- La récupération des données est possible
- Les données renvoyées ont la structure attendue

## Tests de composants

### Test du contexte d'authentification

Le fichier `AuthContext.test.tsx` montre comment tester un contexte React avec des mocks :

```typescript
// Mock complet du contexte d'authentification
jest.mock("../../contexts/AuthContext", () => ({
  useAuth: jest.fn().mockReturnValue({
    userToken: null,
    user: null,
    signIn: jest.fn(),
    signOut: jest.fn(),
    isLoading: false,
  }),
}));

// Importer après le mock
import { useAuth } from "../../contexts/AuthContext";

describe("AuthContext (Mocké)", () => {
  test("permet d'interagir avec les fonctions d'authentification", () => {
    // Accéder au mock pour pouvoir le modifier
    const mockUseAuth = useAuth as jest.Mock;

    // Configuration initiale - non connecté
    mockUseAuth.mockReturnValue({
      userToken: null,
      // ...autres propriétés et méthodes
    });

    // Rendu du composant
    const { getByTestId, rerender } = render(<TestComponent />);

    // Vérifier l'état initial (déconnecté)
    expect(getByTestId("status").props.children).toBe("Déconnecté");

    // Connecter l'utilisateur
    fireEvent.press(getByTestId("login-button"));
    
    // ...reste du test
  });
});
```

### Test d'un écran de connexion

Le fichier `login.test.tsx` montre comment tester un écran avec des mocks pour les dépendances :

```typescript
// Mock du contexte d'authentification
jest.mock("../../contexts/AuthContext", () => ({
  useAuth: () => ({
    signIn: jest.fn(),
    signOut: jest.fn(),
    isLoading: false,
    userToken: null,
    user: null,
  }),
}));

// Mock du router
jest.mock("expo-router", () => ({
  useRouter: () => ({
    push: jest.fn(),
    replace: jest.fn(),
  }),
}));

describe("LoginScreen", () => {
  test("affiche les champs de formulaire", () => {
    const { getByPlaceholderText, getAllByText } = render(<LoginScreen />);

    // Vérifier que les champs de formulaire sont présents
    expect(getByPlaceholderText("Email")).toBeTruthy();
    expect(getByPlaceholderText("Mot de passe")).toBeTruthy();

    // Vérifier que les boutons sont présents
    expect(getAllByText("Connexion").length).toBeGreaterThan(0);
    expect(getAllByText("Scanner un QR Code").length).toBeGreaterThan(0);
  });
});
```

## Tests d'API

Les tests d'API sont conçus pour être exécutés dans un ordre spécifique, car chaque test dépend du résultat du test précédent. Le script `run-api-tests.js` automatise cette exécution séquentielle.

### Utilitaires pour les tests API

Le fichier `api-utils.ts` fournit des fonctions utilitaires pour partager des données entre les tests :

```typescript
import fs from 'fs';
import path from 'path';

// Sauvegarde le token pour les autres tests
export function saveToken(token: string): void {
    fs.writeFileSync(tokenFilePath, token);
}

// Récupère le token sauvegardé
export function getToken(): string {
    if (!fs.existsSync(tokenFilePath)) {
        throw new Error('Le token n\'existe pas. Exécutez d\'abord le test d\'authentification.');
    }
    return fs.readFileSync(tokenFilePath, 'utf8');
}

// ...autres fonctions
```

### Test d'authentification

Le fichier `auth.test.ts` teste le processus d'authentification :

```typescript
describe('API Auth', () => {
    test('login et récupération du token', async () => {
        // Appel API réel pour se connecter
        const response = await fetch('https://keep.kevindupas.com/api/login', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            body: JSON.stringify({
                email: 'test@test.com',
                password: 'password'
            })
        });

        // Vérifier que la requête a réussi
        expect(response.ok).toBe(true);

        // Récupérer le texte brut puis le parser
        const rawText = await response.text();
        const data = JSON.parse(rawText);

        // Vérifier que nous avons un token
        expect(data.access_token).toBeDefined();
        expect(typeof data.access_token).toBe('string');

        // Stocker le token pour les tests suivants
        saveToken(data.access_token);
    }, 15000); // Timeout plus long pour l'appel API réel
});
```

### Test de récupération des notes

Le fichier `notes-list.test.ts` teste la récupération des notes :

```typescript
describe('API Notes List', () => {
    test('récupération des notes avec le token', async () => {
        // Récupérer le token depuis le test précédent
        const authToken = getToken();

        // Appel API pour récupérer les notes
        const response = await fetch('https://keep.kevindupas.com/api/notes', {
            headers: {
                'Authorization': `Bearer ${authToken}`,
                'Accept': 'application/json'
            }
        });

        // Vérifier que la requête a réussi
        expect(response.ok).toBe(true);

        // ...vérifications supplémentaires

        // Si nous avons des notes, sauvegarder l'ID de la première pour le test suivant
        if (notes.length > 0) {
            saveNoteId(notes[0].id);
        } else {
            // Si pas de notes, créer une note pour le test suivant
            // ...code de création d'une note
        }
    }, 15000);
});
```

### Test de récupération d'une note spécifique

Le fichier `note-by-id.test.ts` teste la récupération d'une note par son ID :

```typescript
describe('API Note by ID', () => {
    test('récupération d\'une note spécifique par ID', async () => {
        // Récupérer le token et l'ID de note depuis les tests précédents
        const authToken = getToken();
        const noteId = getNoteId();

        // Appel API pour récupérer une note spécifique
        const response = await fetch(`https://keep.kevindupas.com/api/notes/${noteId}`, {
            headers: {
                'Authorization': `Bearer ${authToken}`,
                'Accept': 'application/json'
            }
        });

        // Vérifier que la requête a réussi
        expect(response.ok).toBe(true);

        // ...vérifications supplémentaires
    }, 15000);
});
```

## Exécution des tests

### Exécution de tous les tests

Pour exécuter tous les tests, utilisez la commande :

```bash
npm test
```

Cette commande lance Jest en mode watch, qui surveille les modifications de fichiers et réexécute les tests affectés.

### Exécution des tests d'API

Pour exécuter spécifiquement les tests d'API dans le bon ordre, utilisez le script dédié :

```bash
npm run test:api
```

Ce script utilise `run-api-tests.js` pour exécuter les tests API dans un ordre spécifique, car ils dépendent les uns des autres.

### Exécution d'un test spécifique

Pour exécuter un test spécifique, vous pouvez utiliser :

```bash
npx jest path/to/your/test.test.tsx
```

Par exemple, pour tester uniquement le contexte d'authentification :

```bash
npx jest __test__/contexts/AuthContext.test.tsx
```

## Bonnes pratiques

### 1. Structure claire et cohérente

Organisez vos tests dans une structure qui reflète celle de votre application, comme nous l'avons fait avec les dossiers `api/`, `components/` et `contexts/`.

### 2. Isolation des tests

Assurez-vous que chaque test est isolé et ne dépend pas de l'état d'autres tests, sauf si explicitement nécessaire comme pour les tests API séquentiels.

### 3. Mocks appropriés

Utilisez des mocks pour les dépendances externes comme `expo-router` et `expo-secure-store` pour que vos tests soient plus rapides et plus fiables.

### 4. Tests complets mais ciblés

Testez les fonctionnalités critiques de votre application, comme l'authentification et les appels API, mais ne testez pas tout. Concentrez-vous sur le code qui est susceptible de contenir des bugs ou qui est critique pour l'application.

### 5. Timeouts adéquats

Pour les tests qui impliquent des appels réseau, utilisez des timeouts plus longs pour éviter les échecs dus à la latence du réseau.

## Résolution des problèmes courants

### Erreurs de transformation

Si vous obtenez des erreurs de transformation, vérifiez votre configuration `transformIgnorePatterns` dans `jest.config.js`. Il est important d'exclure les modules React Native et Expo qui nécessitent une transformation.

### Erreurs de résolution de modules

Si Jest ne trouve pas vos modules, vérifiez votre configuration `moduleNameMapper` dans `jest.config.js` pour vous assurer que les alias de chemins sont correctement définis.

### Erreurs de version de Node Fetch

Si vous obtenez des erreurs avec `node-fetch`, assurez-vous d'utiliser la version 2 (`node-fetch@2`) qui est compatible avec CommonJS.

### Problèmes avec Expo

Si vous avez des problèmes avec les modules Expo, assurez-vous d'utiliser `jest-expo` comme preset dans votre configuration Jest.

### Erreurs liées aux timeouts

Si vos tests API échouent à cause de timeouts, augmentez la valeur du timeout dans la configuration du test :

```typescript
test('votre test', async () => {
  // Votre test
}, 30000); // Augmentez le timeout à 30 secondes
```
