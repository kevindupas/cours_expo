# Guide des React Hooks

Ce guide présente les Hooks essentiels en React, utilisables de la même façon dans ReactJS (web) et React Native (mobile). Les Hooks ont été introduits dans React 16.8 pour permettre l'utilisation d'état et d'autres fonctionnalités React sans écrire de classes.

## Table des matières
- [useState](#usestate)
- [useEffect](#useeffect)
- [useContext](#usecontext)
- [useReducer](#usereducer)
- [useCallback](#usecallback)
- [useMemo](#usememo)
- [useRef](#useref)
- [useLayoutEffect](#uselayouteffect)
- [useImperativeHandle](#useimperativehandle)
- [Custom Hooks](#custom-hooks)
- [Différences entre ReactJS et React Native](#différences-entre-reactjs-et-react-native)
- [Bonnes pratiques](#bonnes-pratiques)

## useState

`useState` permet d'ajouter un état local à un composant fonctionnel.

### Syntaxe de base
```jsx
const [state, setState] = useState(initialState);
```

### Exemples

**Exemple simple avec un compteur :**
```jsx
import React, { useState } from 'react';
import { View, Text, Button } from 'react-native'; // Pour React Native
// import React, { useState } from 'react'; // Pour ReactJS

function Compteur() {
  const [count, setCount] = useState(0);
  
  return (
    <View>
      <Text>Vous avez cliqué {count} fois</Text>
      <Button 
        onPress={() => setCount(count + 1)}
        title="Cliquez-moi"
      />
    </View>
  );
}
```

**Exemple avec un objet :**
```jsx
const [user, setUser] = useState({ name: '', age: 0 });

// Pour mettre à jour partiellement
setUser(prevUser => ({ ...prevUser, name: 'John' }));
```

**Exemple avec le state précédent :**
```jsx
// Utilisation de la valeur précédente (recommandé pour les mises à jour basées sur l'état précédent)
setCount(prevCount => prevCount + 1);
```

**Avec une fonction d'initialisation (lazy initialization) :**
```jsx
// Utile pour les calculs coûteux
const [state, setState] = useState(() => {
  const initialState = calculateInitialState();
  return initialState;
});
```

## useEffect

`useEffect` permet d'exécuter des effets secondaires dans les composants fonctionnels (requêtes réseau, abonnements, manipulations manuelles du DOM, etc.).

### Syntaxe de base
```jsx
useEffect(() => {
  // Code à exécuter après le rendu
  
  // Fonction de nettoyage (optionnelle)
  return () => {
    // Code à exécuter lors du démontage du composant
  };
}, [dependencies]); // Tableau de dépendances
```

### Exemples

**Exécution après chaque rendu :**
```jsx
useEffect(() => {
  console.log('Le composant a été rendu');
});
```

**Exécution une seule fois (équivalent à componentDidMount) :**
```jsx
useEffect(() => {
  console.log('Le composant a été monté');
  
  // Optionnel: fonction de nettoyage
  return () => {
    console.log('Le composant va être démonté');
  };
}, []); // Tableau de dépendances vide
```

**Exécution conditionnelle (lorsque des dépendances changent) :**
```jsx
useEffect(() => {
  console.log(`La valeur count a changé: ${count}`);
}, [count]); // S'exécute uniquement quand count change
```

**Requête API :**
```jsx
useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await fetch('https://api.example.com/data');
      const data = await response.json();
      setData(data);
    } catch (error) {
      console.error('Erreur:', error);
    }
  };
  
  fetchData();
}, []); // Exécuté une fois lors du montage
```

**Abonnement et nettoyage :**
```jsx
useEffect(() => {
  // S'abonner à un événement
  const subscription = someService.subscribe();
  
  // Fonction de nettoyage
  return () => {
    subscription.unsubscribe();
  };
}, [someService]);
```

## useContext

`useContext` permet d'accéder au contexte React sans composant consommateur. Ce Hook est essentiel pour partager des données globales entre des composants sans avoir à passer des props à travers chaque niveau.

### Syntaxe de base
```jsx
const value = useContext(MyContext);
```

### Création et utilisation simple
```jsx
// Création du contexte avec une valeur par défaut
const ThemeContext = React.createContext('light');

// Fournisseur de contexte
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedButton />
    </ThemeContext.Provider>
  );
}

// Utilisation du contexte avec useContext
function ThemedButton() {
  const theme = useContext(ThemeContext);
  
  return (
    <Button 
      title="Bouton Thématique"
      color={theme === 'dark' ? '#333' : '#EEE'} 
    />
  );
}
```

### Exemple détaillé : Gestion d'un thème global

```jsx
import React, { createContext, useContext, useState } from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

// 1. Création du contexte avec un état initial plus complexe
const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => {},
  colors: {
    background: '#FFFFFF',
    text: '#000000',
    primary: '#0066CC',
    secondary: '#6C757D'
  }
});

// 2. Création d'un Hook personnalisé pour faciliter l'utilisation du contexte
function useTheme() {
  const context = useContext(ThemeContext);
  if (context === undefined) {
    throw new Error('useTheme doit être utilisé dans un ThemeProvider');
  }
  return context;
}

// 3. Création du fournisseur de contexte
function ThemeProvider({ children }) {
  // État local pour gérer le thème actuel
  const [currentTheme, setCurrentTheme] = useState('light');
  
  // Définition des couleurs selon le thème
  const colors = {
    light: {
      background: '#FFFFFF',
      text: '#000000',
      primary: '#0066CC',
      secondary: '#6C757D'
    },
    dark: {
      background: '#121212',
      text: '#FFFFFF',
      primary: '#4D9FFF',
      secondary: '#ADB5BD'
    }
  };
  
  // Fonction pour basculer entre les thèmes
  const toggleTheme = () => {
    setCurrentTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };
  
  // Valeur du contexte qui sera fournie
  const value = {
    theme: currentTheme,
    toggleTheme,
    colors: colors[currentTheme]
  };
  
  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// 4. Composants qui utilisent le thème
function ThemedText({ children }) {
  const { colors } = useTheme();
  return (
    <Text style={{ color: colors.text }}>
      {children}
    </Text>
  );
}

function ThemedButton({ title, onPress }) {
  const { colors } = useTheme();
  return (
    <Button
      title={title}
      onPress={onPress}
      color={colors.primary}
    />
  );
}

function ThemeToggle() {
  const { toggleTheme, theme } = useTheme();
  return (
    <Button
      title={`Passer au thème ${theme === 'light' ? 'sombre' : 'clair'}`}
      onPress={toggleTheme}
    />
  );
}

// 5. Composant principal qui utilise les composants thématiques
function HomePage() {
  const { colors } = useTheme();
  
  return (
    <View style={{ 
      flex: 1, 
      backgroundColor: colors.background,
      padding: 20,
      justifyContent: 'center'
    }}>
      <ThemedText>Bienvenue sur notre application!</ThemedText>
      <View style={{ marginVertical: 20 }}>
        <ThemedButton
          title="Continuer"
          onPress={() => console.log('Continuer pressé')}
        />
      </View>
      <ThemeToggle />
    </View>
  );
}

// 6. Application complète avec le fournisseur de thème
export default function App() {
  return (
    <ThemeProvider>
      <HomePage />
    </ThemeProvider>
  );
}

// 7. Utilisation dans d'autres parties de l'application
function ProfileScreen() {
  // On peut accéder au thème depuis n'importe quel composant enfant du ThemeProvider
  const { colors, theme } = useTheme();
  
  return (
    <View style={{ backgroundColor: colors.background, padding: 20 }}>
      <ThemedText>Profil Utilisateur</ThemedText>
      <Text style={{ color: colors.secondary }}>
        Thème actuel: {theme}
      </Text>
      <ThemeToggle />
    </View>
  );
}
```

### Exemple d'utilisation avec plusieurs contextes

```jsx
import React, { createContext, useContext, useState } from 'react';
import { View, Text } from 'react-native';

// Contexte pour l'authentification
const AuthContext = createContext(null);

// Contexte pour les préférences utilisateur
const PreferencesContext = createContext(null);

// Fournisseurs combinés
function AppProviders({ children }) {
  const [user, setUser] = useState(null);
  const [preferences, setPreferences] = useState({
    notifications: true,
    darkMode: false
  });
  
  const login = (userData) => {
    setUser(userData);
  };
  
  const logout = () => {
    setUser(null);
  };
  
  const updatePreferences = (newPrefs) => {
    setPreferences(prev => ({ ...prev, ...newPrefs }));
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      <PreferencesContext.Provider value={{ preferences, updatePreferences }}>
        {children}
      </PreferencesContext.Provider>
    </AuthContext.Provider>
  );
}

// Hooks personnalisés pour faciliter l'utilisation
function useAuth() {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth doit être utilisé dans un AuthProvider');
  }
  return context;
}

function usePreferences() {
  const context = useContext(PreferencesContext);
  if (context === undefined) {
    throw new Error('usePreferences doit être utilisé dans un PreferencesProvider');
  }
  return context;
}

// Exemple d'utilisation dans un composant
function UserDashboard() {
  const { user, logout } = useAuth();
  const { preferences, updatePreferences } = usePreferences();
  
  if (!user) {
    return <Text>Veuillez vous connecter</Text>;
  }
  
  return (
    <View>
      <Text>Bienvenue, {user.name}</Text>
      <Text>Notifications: {preferences.notifications ? 'Activées' : 'Désactivées'}</Text>
      <Button 
        title="Activer/Désactiver les notifications" 
        onPress={() => updatePreferences({ 
          notifications: !preferences.notifications 
        })} 
      />
      <Button title="Déconnexion" onPress={logout} />
    </View>
  );
}

// Application complète
export default function App() {
  return (
    <AppProviders>
      <UserDashboard />
    </AppProviders>
  );
}
```

### Bonnes pratiques pour useContext

1. **Séparation des contextes** : Créez plusieurs contextes pour différentes préoccupations plutôt qu'un seul grand contexte
2. **Hooks personnalisés** : Encapsulez l'utilisation de useContext dans des hooks personnalisés
3. **Vérification d'existence** : Vérifiez si le contexte existe avant de l'utiliser
4. **Optimisation des rendus** : Séparez les contextes de données fréquemment modifiées de ceux qui changent rarement
5. **Valeur par défaut sensée** : Fournissez des valeurs par défaut utiles lors de la création du contexte

## useReducer

`useReducer` est une alternative à `useState` pour gérer des états complexes et des logiques de mise à jour.

### Syntaxe de base
```jsx
const [state, dispatch] = useReducer(reducer, initialState, init);
```

### Exemple
```jsx
import React, { useReducer } from 'react';
import { View, Text, Button } from 'react-native';

// Définition du reducer
function counterReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });
  
  return (
    <View>
      <Text>Compteur: {state.count}</Text>
      <Button title="+" onPress={() => dispatch({ type: 'increment' })} />
      <Button title="-" onPress={() => dispatch({ type: 'decrement' })} />
      <Button title="Reset" onPress={() => dispatch({ type: 'reset' })} />
    </View>
  );
}
```

## useCallback

`useCallback` retourne une version mémorisée d'une fonction callback qui ne change que si ses dépendances changent.

### Syntaxe de base
```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b], // Dépendances
);
```

### Exemple
```jsx
import React, { useState, useCallback } from 'react';
import { View, Button } from 'react-native';

function ParentComponent() {
  const [count, setCount] = useState(0);
  
  // Cette fonction est mémorisée et ne sera pas recréée à chaque rendu
  const incrementCount = useCallback(() => {
    setCount(prevCount => prevCount + 1);
  }, []); // Pas de dépendances, la fonction ne change jamais
  
  return (
    <View>
      <Text>Count: {count}</Text>
      <ChildComponent onIncrement={incrementCount} />
    </View>
  );
}

// ChildComponent reçoit la fonction mémorisée
function ChildComponent({ onIncrement }) {
  console.log('Child rendered');
  return <Button title="Increment" onPress={onIncrement} />;
}

// Optimisation: éviter les rendus inutiles du composant enfant
const MemoizedChildComponent = React.memo(ChildComponent);
```

## useMemo

`useMemo` mémorise le résultat d'un calcul coûteux, qui ne sera recalculé que si les dépendances changent.

### Syntaxe de base
```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

### Exemple
```jsx
import React, { useState, useMemo } from 'react';
import { View, Text, TextInput } from 'react-native';

function ExpensiveCalculation() {
  const [numbers, setNumbers] = useState('');
  
  // Calcul coûteux mémorisé
  const sortedNumbers = useMemo(() => {
    console.log('Calcul en cours...');
    if (!numbers) return [];
    
    // Convertir la chaîne en tableau de nombres
    const numArray = numbers.split(',').map(num => parseInt(num.trim(), 10));
    
    // Tri (simulant un calcul coûteux)
    return [...numArray].sort((a, b) => a - b);
  }, [numbers]); // Recalculé uniquement quand numbers change
  
  return (
    <View>
      <TextInput
        value={numbers}
        onChangeText={setNumbers}
        placeholder="Entrez des nombres séparés par des virgules"
      />
      <Text>Nombres triés: {sortedNumbers.join(', ')}</Text>
    </View>
  );
}
```

## useRef

`useRef` crée un objet qui persiste à travers les rendus et dont la mutation ne déclenche pas de re-rendu.

### Syntaxe de base
```jsx
const refContainer = useRef(initialValue);
```

### Exemples

**Accès direct à un élément DOM/natif :**
```jsx
import React, { useRef, useEffect } from 'react';
import { View, TextInput, Button } from 'react-native';

function TextInputWithFocus() {
  const inputRef = useRef(null);
  
  const focusInput = () => {
    // Accès à la méthode focus() du TextInput
    inputRef.current.focus();
  };
  
  return (
    <View>
      <TextInput
        ref={inputRef}
        placeholder="Tapez ici"
      />
      <Button title="Focus" onPress={focusInput} />
    </View>
  );
}
```

**Stockage de valeurs persistantes (sans déclencher de re-rendu) :**
```jsx
function Timer() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef(null);
  
  useEffect(() => {
    intervalRef.current = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
    
    return () => clearInterval(intervalRef.current);
  }, []);
  
  const stopTimer = () => {
    clearInterval(intervalRef.current);
  };
  
  return (
    <View>
      <Text>Timer: {count}</Text>
      <Button title="Stop" onPress={stopTimer} />
    </View>
  );
}
```

## useLayoutEffect

`useLayoutEffect` est identique à `useEffect`, mais il s'exécute de façon synchrone après toutes les mutations DOM/UI et avant que le navigateur n'affiche les changements.

### Utilisation
```jsx
useLayoutEffect(() => {
  // Code exécuté synchronement après les mutations DOM/UI
  // mais avant l'affichage à l'écran
  
  return () => {
    // Nettoyage
  };
}, [dependencies]);
```

### Exemple
```jsx
import React, { useState, useLayoutEffect, useRef } from 'react';
import { View, Text, Button } from 'react-native';

function MeasureExample() {
  const [dimensions, setDimensions] = useState({ width: 0, height: 0 });
  const elementRef = useRef(null);
  
  useLayoutEffect(() => {
    if (elementRef.current) {
      // Dans React Native, utiliser les méthodes de mesure natives
      elementRef.current.measure((x, y, width, height) => {
        setDimensions({ width, height });
      });
    }
  }, []);
  
  return (
    <View>
      <View
        ref={elementRef}
        style={{ backgroundColor: 'red', padding: 20 }}
      >
        <Text>Mesurez-moi</Text>
      </View>
      <Text>Largeur: {dimensions.width}, Hauteur: {dimensions.height}</Text>
    </View>
  );
}
```

## useImperativeHandle

`useImperativeHandle` personnalise l'instance qui est exposée lors de l'utilisation de refs.

### Syntaxe de base
```jsx
useImperativeHandle(ref, createHandle, [deps]);
```

### Exemple
```jsx
import React, { useRef, useImperativeHandle, forwardRef } from 'react';
import { View, TextInput, Button } from 'react-native';

// Composant avec ref transmise
const CustomInput = forwardRef((props, ref) => {
  const inputRef = useRef(null);
  
  // Exposer des méthodes personnalisées via la ref
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    blur: () => {
      inputRef.current.blur();
    },
    clear: () => {
      inputRef.current.clear();
    }
  }));
  
  return <TextInput ref={inputRef} {...props} />;
});

// Composant parent utilisant CustomInput
function ParentComponent() {
  const inputRef = useRef(null);
  
  return (
    <View>
      <CustomInput 
        ref={inputRef}
        placeholder="Texte personnalisé" 
      />
      <Button 
        title="Focus" 
        onPress={() => inputRef.current.focus()} 
      />
      <Button 
        title="Clear" 
        onPress={() => inputRef.current.clear()} 
      />
    </View>
  );
}
```

## Custom Hooks

Les Hooks personnalisés permettent d'extraire la logique des composants dans des fonctions réutilisables.

### Exemple: Hook de formulaire
```jsx
import { useState } from 'react';

// Hook personnalisé pour gérer les formulaires
function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);
  
  const handleChange = (name, value) => {
    setValues(prevValues => ({
      ...prevValues,
      [name]: value
    }));
  };
  
  const resetForm = () => {
    setValues(initialValues);
  };
  
  return {
    values,
    handleChange,
    resetForm
  };
}

// Utilisation du hook personnalisé
function LoginForm() {
  const { values, handleChange, resetForm } = useForm({
    username: '',
    password: ''
  });
  
  const handleSubmit = () => {
    console.log('Valeurs soumises:', values);
    // Logique d'authentification
    resetForm();
  };
  
  return (
    <View>
      <TextInput
        value={values.username}
        onChangeText={(text) => handleChange('username', text)}
        placeholder="Nom d'utilisateur"
      />
      <TextInput
        value={values.password}
        onChangeText={(text) => handleChange('password', text)}
        placeholder="Mot de passe"
        secureTextEntry
      />
      <Button title="Connexion" onPress={handleSubmit} />
    </View>
  );
}
```

### Exemple: Hook pour API
```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const result = await response.json();
        setData(result);
      } catch (error) {
        setError(error.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [url]);
  
  return { data, loading, error };
}

// Utilisation
function UserProfile() {
  const { data, loading, error } = useFetch('https://api.example.com/user/1');
  
  if (loading) return <Text>Chargement...</Text>;
  if (error) return <Text>Erreur: {error}</Text>;
  
  return (
    <View>
      <Text>Nom: {data.name}</Text>
      <Text>Email: {data.email}</Text>
    </View>
  );
}
```

## Différences entre ReactJS et React Native

Les Hooks fonctionnent de manière identique entre ReactJS et React Native, mais il existe quelques différences à noter :

1. **Références DOM vs Natives :**
   - Dans ReactJS, `useRef` accède aux éléments DOM
   - Dans React Native, `useRef` accède aux composants natifs

2. **Animations :**
   - React Native utilise souvent `Animated` avec `useRef` pour les animations
   - ReactJS utilise CSS ou des bibliothèques comme Framer Motion

3. **Mesures d'éléments :**
   - React Native : `elementRef.current.measure(callback)`
   - ReactJS : `elementRef.current.getBoundingClientRect()`

4. **Gestion du cycle de vie et des interactions plateforme :**
   - React Native nécessite des hooks spécifiques pour interagir avec les APIs natives (AppState, BackHandler, etc.)

## Bonnes pratiques

1. **Règles des Hooks :**
   - N'appelez les Hooks qu'au niveau supérieur, jamais dans des conditions, boucles ou fonctions imbriquées
   - N'appelez les Hooks que depuis des composants fonctionnels React ou des Hooks personnalisés

2. **Optimisation des performances :**
   - Utilisez `React.memo` pour éviter les rendus inutiles des composants
   - Utilisez `useCallback` pour mémoriser les fonctions passées comme props
   - Utilisez `useMemo` pour mémoriser les calculs coûteux

3. **Gestion des dépendances :**
   - Spécifiez toujours correctement les dépendances dans le tableau de `useEffect`, `useCallback` et `useMemo`
   - Utilisez le linter ESLint avec le plugin `eslint-plugin-react-hooks` pour éviter les erreurs

4. **Hooks personnalisés :**
   - Créez des Hooks personnalisés pour réutiliser la logique entre composants
   - Préfixez toujours les Hooks personnalisés par "use" (convention)

5. **Tests :**
   - Testez vos Hooks avec `@testing-library/react-hooks` (pour ReactJS)
   - Isolez la logique métier dans des Hooks pour faciliter les tests

## Conclusion

Les React Hooks offrent une approche plus simple et plus flexible pour gérer l'état et les effets secondaires dans les applications React, qu'elles soient web (ReactJS) ou mobiles (React Native). En comprenant bien ces Hooks fondamentaux et en suivant les bonnes pratiques, vous pouvez créer des composants plus lisibles, plus maintenables et plus performants.

N'oubliez pas que la documentation officielle de React est une ressource précieuse pour approfondir votre compréhension des Hooks : [React Hooks Documentation](https://reactjs.org/docs/hooks-intro.html).
