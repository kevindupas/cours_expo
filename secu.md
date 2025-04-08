# Étude de cas : Sécurisation d'une application de gestion de mots de passe en Expo

## Contexte

Vous travaillez sur une application mobile de gestion de mots de passe appelée "SecurePass" développée avec React Native et Expo. Cette application permet aux utilisateurs de stocker leurs identifiants pour différents sites web et services.

Malheureusement, l'équipe précédente a développé une version fonctionnelle mais qui présente plusieurs problèmes de sécurité critiques. Le gestionnaire de projet vous a assigné la tâche de sécuriser l'application en utilisant les bonnes pratiques et les modules de sécurité d'Expo.

## Objectifs d'apprentissage

- Comprendre les risques associés au stockage non sécurisé des données sensibles
- Utiliser `expo-secure-store` pour le stockage sécurisé des informations sensibles
- Implémenter la génération aléatoire sécurisée avec `expo-random`
- Créer un système de chiffrement/déchiffrement pour les données sensibles
- Appliquer les bonnes pratiques de l'OWASP Mobile Top 10

## Composant vulnérable à sécuriser

Le composant principal à sécuriser est le gestionnaire de mots de passe qui stocke actuellement toutes les informations en texte clair dans AsyncStorage. De plus, il génère des mots de passe avec `Math.random()`, ce qui n'est pas cryptographiquement sécurisé.

Voici le code actuel du composant `PasswordManager.tsx` :

```tsx
import React, { useState, useEffect } from 'react';
import { View, Text, TextInput, Button, FlatList, StyleSheet, Alert } from 'react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';

// Interface pour les enregistrements de mots de passe
interface PasswordEntry {
  id: string;
  website: string;
  username: string;
  password: string;
}

export default function PasswordManager() {
  const [entries, setEntries] = useState<PasswordEntry[]>([]);
  const [website, setWebsite] = useState('');
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  // Chargement des mots de passe depuis AsyncStorage
  useEffect(() => {
    const loadPasswords = async () => {
      try {
        const savedPasswords = await AsyncStorage.getItem('passwords');
        if (savedPasswords) {
          setEntries(JSON.parse(savedPasswords));
        }
      } catch (error) {
        console.error('Erreur lors du chargement des mots de passe:', error);
      }
    };

    loadPasswords();
  }, []);

  // Sauvegarde des mots de passe dans AsyncStorage
  const savePasswords = async (updatedEntries: PasswordEntry[]) => {
    try {
      await AsyncStorage.setItem('passwords', JSON.stringify(updatedEntries));
    } catch (error) {
      console.error('Erreur lors de la sauvegarde des mots de passe:', error);
    }
  };

  // Ajout d'un nouveau mot de passe
  const addPassword = () => {
    if (!website || !username || !password) {
      Alert.alert('Champs requis', 'Veuillez remplir tous les champs');
      return;
    }

    const newEntry: PasswordEntry = {
      id: Date.now().toString(),
      website,
      username,
      password,
    };

    const updatedEntries = [...entries, newEntry];
    setEntries(updatedEntries);
    savePasswords(updatedEntries);

    // Réinitialisation des champs
    setWebsite('');
    setUsername('');
    setPassword('');
  };

  // Suppression d'un mot de passe
  const deletePassword = (id: string) => {
    const updatedEntries = entries.filter(entry => entry.id !== id);
    setEntries(updatedEntries);
    savePasswords(updatedEntries);
  };

  // Génération d'un mot de passe aléatoire (non sécurisé)
  const generatePassword = () => {
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()';
    let generatedPassword = '';
    
    for (let i = 0; i < 12; i++) {
      const randomIndex = Math.floor(Math.random() * chars.length);
      generatedPassword += chars[randomIndex];
    }
    
    setPassword(generatedPassword);
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Gestionnaire de mots de passe</Text>
      
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Site web"
          value={website}
          onChangeText={setWebsite}
        />
        <TextInput
          style={styles.input}
          placeholder="Nom d'utilisateur"
          value={username}
          onChangeText={setUsername}
        />
        <View style={styles.passwordRow}>
          <TextInput
            style={[styles.input, { flex: 1 }]}
            placeholder="Mot de passe"
            value={password}
            onChangeText={setPassword}
            secureTextEntry
          />
          <Button title="Générer" onPress={generatePassword} />
        </View>
        <Button title="Ajouter" onPress={addPassword} />
      </View>

      <FlatList
        data={entries}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.entryItem}>
            <View>
              <Text style={styles.website}>{item.website}</Text>
              <Text>Utilisateur: {item.username}</Text>
              <Text>Mot de passe: {item.password}</Text>
            </View>
            <Button title="Supprimer" onPress={() => deletePassword(item.id)} color="red" />
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  inputContainer: {
    marginBottom: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    borderRadius: 5,
    padding: 10,
    marginBottom: 10,
  },
  passwordRow: {
    flexDirection: 'row',
    marginBottom: 10,
  },
  entryItem: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 15,
    borderBottomWidth: 1,
    borderBottomColor: '#ccc',
  },
  website: {
    fontWeight: 'bold',
    fontSize: 16,
  },
});
```

## Problèmes de sécurité à résoudre

1. **Stockage non sécurisé des mots de passe** (OWASP Mobile Top 10: M2)
   - Les mots de passe sont stockés en texte clair dans AsyncStorage
   - AsyncStorage n'est pas chiffré et peut être accessible sur les appareils rootés/jailbreakés

2. **Génération de mots de passe faible** (OWASP Mobile Top 10: M5)
   - Utilisation de `Math.random()` qui n'est pas cryptographiquement sécurisé
   - Les mots de passe générés sont prévisibles et vulnérables

3. **Absence d'authentification locale** (OWASP Mobile Top 10: M4)
   - Aucune protection pour accéder aux mots de passe stockés (PIN, biométrie, etc.)

## Tâches à réaliser

1. **Installer les packages requis**
   ```bash
   npx expo install expo-secure-store expo-random
   ```

2. **Sécuriser le stockage des mots de passe**
   - Remplacer AsyncStorage par expo-secure-store
   - Implémenter un chiffrement supplémentaire des mots de passe avant stockage

3. **Améliorer la génération de mots de passe**
   - Utiliser expo-random pour générer des nombres aléatoires cryptographiquement sécurisés
   - Renforcer la complexité des mots de passe générés

4. **Implémenter une protection par mot de passe maître ou authentification biométrique**
   - Ajouter une vérification avant d'afficher les mots de passe stockés

## Indications techniques

### Utilisation d'expo-secure-store
- expo-secure-store utilise le Keychain sur iOS et le Keystore sur Android
- Stockage des données sous forme de paires clé-valeur chiffrées
- Limitations: 2048 octets par valeur, donc pour des données plus grandes, il faudra les diviser

```typescript
import * as SecureStore from 'expo-secure-store';

// Exemple de stockage
await SecureStore.setItemAsync(key, value, options);

// Exemple de récupération
const result = await SecureStore.getItemAsync(key, options);
```

### Utilisation d'expo-random
- Génère des nombres aléatoires cryptographiquement sécurisés
- Plus fiable que Math.random() pour les besoins de sécurité

```typescript
import * as Random from 'expo-random';

// Génération d'octets aléatoires
const randomBytes = await Random.getRandomBytesAsync(16);
```

### Chiffrement supplémentaire
- Vous pouvez implémenter un chiffrement AES en utilisant un mot de passe maître
- Vous pouvez utiliser des bibliothèques comme CryptoJS pour le chiffrement

## Livrables attendus

1. Un composant `PasswordManager.tsx` sécurisé qui utilise expo-secure-store et expo-random
2. Un README expliquant les améliorations de sécurité apportées
3. (Optionnel) Des tests unitaires validant la sécurité

## Critères d'évaluation

- Utilisation correcte d'expo-secure-store pour le stockage sécurisé
- Implémentation d'une génération de mots de passe sécurisée avec expo-random
- Protection des données sensibles avec un chiffrement supplémentaire
- Respect des bonnes pratiques de sécurité mobile OWASP
- Qualité et lisibilité du code
- Documentation des choix de sécurité

---

### Exemple de solution pour la génération de mots de passe sécurisés

Voici un exemple de code pour la génération de mots de passe cryptographiquement sécurisés utilisant expo-random :

```typescript
import * as Random from 'expo-random';

const generateSecurePassword = async (length = 16) => {
  // Définir les caractères possibles
  const charset = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()';
  
  // Obtenir des octets aléatoires
  const randomBytes = await Random.getRandomBytesAsync(length);
  
  // Convertir les octets en caractères du charset
  let password = '';
  for (let i = 0; i < length; i++) {
    // Utiliser le modulo pour rester dans les limites du charset
    const randomIndex = randomBytes[i] % charset.length;
    password += charset[randomIndex];
  }
  
  return password;
};
```

Bon courage pour votre exercice de sécurisation !
