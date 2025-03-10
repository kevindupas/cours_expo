# Guide des Composants React Native

## Composants de Base

### View
```jsx
<View style={styles.container}>
  {/* Contenu */}
</View>
```
Le composant fondamental pour construire une interface utilisateur. Équivalent à une `<div>` en web, il s'agit d'un conteneur qui prend en charge la mise en page avec Flexbox, le style, la gestion des événements tactiles et l'accessibilité.

### Text
```jsx
<Text style={styles.text}>Bonjour le monde</Text>
```
Composant pour afficher du texte. Tous les éléments textuels dans React Native doivent être encapsulés dans un composant `<Text>`.

### Image
```jsx
<Image
  source={require('./mon-image.png')}
  // ou source={{uri: 'https://example.com/image.jpg'}}
  style={styles.image}
/>
```
Affiche différents types d'images, y compris des images statiques, des images réseau et des images dans le système de fichiers local.

### TextInput
```jsx
<TextInput
  style={styles.input}
  placeholder="Saisissez du texte"
  onChangeText={text => setTexte(text)}
  value={texte}
/>
```
Permet à l'utilisateur de saisir du texte. Il peut être configuré pour de nombreux types de saisie de texte comme le texte simple, les e-mails, les mots de passe, etc.

### ScrollView
```jsx
<ScrollView>
  {/* Contenu défilable */}
</ScrollView>
```
Conteneur qui permet de faire défiler plusieurs composants et vues. Il prend en charge le défilement vertical et horizontal.

### FlatList
```jsx
<FlatList
  data={donnees}
  renderItem={({ item }) => <Item titre={item.titre} />}
  keyExtractor={item => item.id}
/>
```
Un composant performant pour afficher de longues listes de données qui peuvent changer ou être défilées. Il n'affiche que les éléments actuellement visibles à l'écran.

### SectionList
```jsx
<SectionList
  sections={DATA}
  keyExtractor={(item, index) => item + index}
  renderItem={({ item }) => <Item title={item} />}
  renderSectionHeader={({ section: { title } }) => (
    <Text style={styles.header}>{title}</Text>
  )}
/>
```
Comme FlatList, mais pour afficher des données sectionnées comme une liste de contacts avec des en-têtes alphabétiques.

## Composants d'Interface Utilisateur

### Button
```jsx
<Button
  title="Appuyez ici"
  onPress={() => alert('Bouton pressé!')}
  color="#841584"
/>
```
Un bouton simple qui affiche un titre et qui réagit aux pressions.

### TouchableOpacity
```jsx
<TouchableOpacity onPress={() => alert('Pressé!')}>
  <Text>Appuyez ici</Text>
</TouchableOpacity>
```
Un wrapper qui rend ses enfants réactifs aux touchers et fournit un retour visuel en réduisant l'opacité lors du toucher.

### TouchableHighlight
```jsx
<TouchableHighlight
  onPress={() => alert('Pressé!')}
  underlayColor="#DDDDDD"
>
  <View style={styles.button}>
    <Text>Appuyez ici</Text>
  </View>
</TouchableHighlight>
```
Similaire à TouchableOpacity, mais change la couleur de fond lors du toucher.

### Switch
```jsx
<Switch
  trackColor={{ false: "#767577", true: "#81b0ff" }}
  thumbColor={isEnabled ? "#f5dd4b" : "#f4f3f4"}
  onValueChange={toggleSwitch}
  value={isEnabled}
/>
```
Affiche un contrôle booléen d'entrée (interrupteur on/off).

### Slider
```jsx
<Slider
  style={styles.slider}
  minimumValue={0}
  maximumValue={1}
  minimumTrackTintColor="#FFFFFF"
  maximumTrackTintColor="#000000"
  onValueChange={valeur => setValeur(valeur)}
/>
```
Un composant qui permet aux utilisateurs de sélectionner une valeur dans une plage de valeurs en déplaçant un curseur.

### ActivityIndicator
```jsx
<ActivityIndicator size="large" color="#0000ff" />
```
Affiche un indicateur de chargement tournant (spinner).

## Composants pour la Mise en Page

### SafeAreaView
```jsx
<SafeAreaView style={styles.container}>
  {/* Contenu */}
</SafeAreaView>
```
Rend le contenu dans la zone sûre d'un appareil (évite les encoches et les bords arrondis sur certains appareils).

### KeyboardAvoidingView
```jsx
<KeyboardAvoidingView
  behavior={Platform.OS === "ios" ? "padding" : "height"}
  style={styles.container}
>
  {/* Contenu */}
</KeyboardAvoidingView>
```
Ajuste automatiquement sa hauteur ou sa position en fonction de la hauteur du clavier virtuel.

### Modal
```jsx
<Modal
  animationType="slide"
  transparent={true}
  visible={modalVisible}
  onRequestClose={() => {
    setModalVisible(!modalVisible);
  }}
>
  {/* Contenu du modal */}
</Modal>
```
Un composant qui apparaît au-dessus de l'autre contenu pour fournir une information ou une interaction dédiée.

### StatusBar
```jsx
<StatusBar barStyle="dark-content" />
```
Contrôle l'apparence de la barre d'état de l'application.

## Navigation

### React Navigation (bibliothèque externe)

```jsx
// Installation
// npm install @react-navigation/native @react-navigation/stack

// Exemple de base
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Accueil" component={AccueilEcran} />
        <Stack.Screen name="Details" component={DetailsEcran} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
React Navigation est la bibliothèque de navigation la plus populaire pour React Native. Elle fournit des navigateurs stack, tab, drawer et plus encore.

## Composants pour les Entrées Spécifiques

### Picker (désormais dans @react-native-picker/picker)
```jsx
import {Picker} from '@react-native-picker/picker';

<Picker
  selectedValue={selectedLanguage}
  onValueChange={(itemValue, itemIndex) =>
    setSelectedLanguage(itemValue)
  }>
  <Picker.Item label="Java" value="java" />
  <Picker.Item label="JavaScript" value="js" />
</Picker>
```
Un composant qui permet à l'utilisateur de sélectionner un élément dans une liste déroulante.

### DateTimePicker (bibliothèque externe)
```jsx
import DateTimePicker from '@react-native-community/datetimepicker';

<DateTimePicker
  value={date}
  mode={'date'}
  display="default"
  onChange={onChange}
/>
```
Un composant pour sélectionner une date ou une heure.

## Composants pour le Stockage de Données

### AsyncStorage (désormais dans @react-native-async-storage/async-storage)
```jsx
import AsyncStorage from '@react-native-async-storage/async-storage';

// Stockage de données
const storeData = async (value) => {
  try {
    await AsyncStorage.setItem('@storage_Key', value)
  } catch (e) {
    // erreur de sauvegarde
  }
}

// Récupération de données
const getData = async () => {
  try {
    const value = await AsyncStorage.getItem('@storage_Key')
    if(value !== null) {
      // la valeur existe
    }
  } catch(e) {
    // erreur de lecture
  }
}
```
Un système de stockage clé-valeur simple, asynchrone et non chiffré pour les applications React Native.

## Composants d'Interaction avec l'Appareil

### Alert
```jsx
Alert.alert(
  "Titre",
  "Message",
  [
    {
      text: "Annuler",
      onPress: () => console.log("Annuler pressé"),
      style: "cancel"
    },
    { text: "OK", onPress: () => console.log("OK pressé") }
  ]
);
```
Affiche une boîte de dialogue d'alerte ou d'invite.

### Linking
```jsx
import { Linking } from 'react-native';

// Ouvrir une URL
Linking.openURL('https://example.com');

// Ouvrir des applications natives
Linking.openURL('tel:+33123456789'); // Téléphone
Linking.openURL('sms:+33123456789'); // SMS
Linking.openURL('mailto:contact@example.com'); // Email
```
API pour interagir avec les applications ou URLs externes.

### Vibration
```jsx
import { Vibration } from 'react-native';

// Vibrer une fois
Vibration.vibrate();

// Vibrer avec un pattern
Vibration.vibrate([1000, 500, 1000, 500]);
```
API de vibration de l'appareil.

## Bonnes Pratiques de Développement

### Fragment
```jsx
<>
  <Text>Élément 1</Text>
  <Text>Élément 2</Text>
</>
```
Pas vraiment un composant, mais une syntaxe qui permet de retourner plusieurs éléments sans avoir à les envelopper dans un conteneur supplémentaire.

### StyleSheet
```jsx
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  text: {
    fontSize: 20,
    fontWeight: 'bold',
  }
});
```
API pour créer des styles d'une manière qui optimise les performances.

## Considérations pour les Applications Professionnelles

### React Native Web
Permet d'utiliser les composants React Native sur le web, offrant un code partagé entre les plateformes mobiles et web.

### React Native Gesture Handler
```jsx
import { RectButton } from 'react-native-gesture-handler';

<RectButton
  onPress={() => console.log('Pressé!')}
  style={styles.button}
>
  <Text>Bouton avec geste natif</Text>
</RectButton>
```
Fournit des API natives pour la gestion des gestes, remplaçant le système de gestion des événements tactiles de React Native.

### React Native Reanimated
```jsx
import Animated from 'react-native-reanimated';

<Animated.View
  style={{
    opacity: fadeAnim,
    transform: [{ translateY: slideAnim }],
  }}
>
  <Text>Contenu animé</Text>
</Animated.View>
```
Bibliothèque pour créer des animations fluides et des interactions tactiles.

## Conclusion

React Native offre un large éventail de composants pour développer des applications mobiles cross-platform. Cette liste n'est pas exhaustive, car l'écosystème React Native est en constante évolution avec de nouvelles bibliothèques et composants qui émergent régulièrement. Pour des informations à jour, consultez la [documentation officielle de React Native](https://reactnative.dev/docs/components-and-apis).
