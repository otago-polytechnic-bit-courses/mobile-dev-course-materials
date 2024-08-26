# 05: Firebase

## Getting Started

Create a new project using the following command:

```bash
npx create-expo-app 05-playground --template blank
```

Install the following dependencies:

```bash
npm install dotenv expo-constants firebase
```

- `dotenv` - loads environment variables from a `.env` file into `process.env`
- `expo-constants` - provides system information, i.e., device name, operating system, etc.
- `firebase` - provides access to Firebase services, i.e., Firestore database, Authentication, etc.

## Firebase Setup

1. Go to [https://firebase.google.com/](https://firebase.google.com/) and click on "Create a project".

![](<../resources%20(ignore)/img/06/web-1.png>)

2. Name your project, check the box to accept the terms, and click on "Continue".

![](<../resources%20(ignore)/img/06/web-2.png>)

3. Disable Google Analytics and click on "Create project".

![](<../resources%20(ignore)/img/06/web-3.png>)

4. Wait for the project to provision.

![](<../resources%20(ignore)/img/06/web-4.png>)

5. Click on "Continue".

![](<../resources%20(ignore)/img/06/web-5.png>)

6. You will see five circle icons - iOS, Android, Web, Unity, and Flutter. Select the "Web" icon. **Note:** That is the third icon.

![](<../resources%20(ignore)/img/06/web-6.png>)

7. Name your app and click on "Register app".

![](<../resources%20(ignore)/img/06/web-7.png>)

8. In the root directory, create a new directory called `config`. In the `config` directory, create a new file called `firebase.js`. Copy the code from the "Firebase SDK snippet" and paste it into the `firebase.js` file. **Note:** You will modify this code later. Click on "Continue to console".

![](<../resources%20(ignore)/img/06/web-8.png>)

9. Click on "Firestore Database".

![](<../resources%20(ignore)/img/06/web-9.png>)

10. Click on "Create database".

![](<../resources%20(ignore)/img/06/web-10.png>)

11. Select either "Start in test mode" or "Start in production mode". Click on "Next".

![](<../resources%20(ignore)/img/06/web-11.png>)

12. Select a location and click on "Enable".

![](<../resources%20(ignore)/img/06/web-12.png>)

13. Wait for **Firestore database** to provision.

![](<../resources%20(ignore)/img/06/web-13.png>)

14. You should see the following:

![](<../resources%20(ignore)/img/06/web-14.png>)

15. If you selected "Start in production mode", you will see the following:

![](<../resources%20(ignore)/img/06/web-15.png>)

16. Change the rules to the following:

```js
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write;
    }
  }
}
```

Click on "Publish".

![](<../resources%20(ignore)/img/06/web-16.png>)

## Environment Variables

In the root directory, create a new file called `.env`. Add the following environment variables:

```bash
API_KEY=<your-api-key>
AUTH_DOMAIN=<your-auth-domain>
PROJECT_ID=<your-project-id>
STORAGE_BUCKET=<your-storage-bucket>
MESSAGING_SENDER_ID=<your-messaging-sender-id>
APP_ID=<your-app-id>
```

Remember to replace the placeholder values with your own. Also, create a `.example.env` file. This file should contain the same environment variables as the `.env` file, but with the placeholder values as shown above. This file will be used as a reference for other developers.

## App Setup

In the root directory, rename the `app.json` file to `app.config.js`. In the `app.config.js` file, replace the code with the following:

```js
import "dotenv/config"; // Loads environment variables from a .env file into process.env

export default {
  expo: {
    name: "05-playground",
    slug: "05-playground",
    version: "1.0.0",
    orientation: "portrait",
    icon: "./assets/icon.png",
    userInterfaceStyle: "light",
    splash: {
      image: "./assets/splash.png",
      resizeMode: "contain",
      backgroundColor: "#ffffff",
    },
    assetBundlePatterns: ["**/*"],
    ios: {
      supportsTablet: true,
    },
    android: {
      adaptiveIcon: {
        foregroundImage: "./assets/adaptive-icon.png",
        backgroundColor: "#ffffff",
      },
    },
    web: {
      favicon: "./assets/favicon.png",
    },
    extra: {
      // Environment variables
      apiKey: process.env.API_KEY,
      authDomain: process.env.AUTH_DOMAIN,
      projectId: process.env.PROJECT_ID,
      storageBucket: process.env.STORAGE_BUCKET,
      messagingSenderId: process.env.MESSAGING_SENDER_ID,
      appId: process.env.APP_ID,
    },
  },
};
```

### config/firebase.js

In the `firebase.js` file, replace the code with the following:

```js
import Constants from "expo-constants";
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: Constants.expoConfig.extra.apiKey,
  authDomain: Constants.expoConfig.extra.authDomain,
  projectId: Constants.expoConfig.extra.projectId,
  storageBucket: Constants.expoConfig.extra.storageBucket,
  messagingSenderId: Constants.expoConfig.extra.messagingSenderId,
  appId: Constants.expoConfig.extra.appId,
};

initializeApp(firebaseConfig); // Initialize Firebase

const database = getFirestore();

export default database; // Get a Firestore instance
```

### App.jsx

In the `App.jsx` file, replace the code with the following:

```jsx
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";

import HomeScreen from "./screens/HomeScreen";
import AddProductScreen from "./screens/AddProductScreen";

const Stack = createNativeStackNavigator();

const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="AddProduct" component={AddProductScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
};

export default App;
```

You will create the `HomeScreen` and `AddProductScreen` screens soon.

### HomeScreen.jsx

In the root directory, create a new directory called `screens`. In the `screens` directory, create a new file called `HomeScreen.jsx`. Add the following code:

```jsx
import { Button, View, Text } from "react-native";

const HomeScreen = (props) => {
  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text>Home Screen</Text>
      <Button
        title="Add Product"
        onPress={() => props.navigation.navigate("AddProduct")}
      />
    </View>
  );
};

export default HomeScreen;
```

Reload your application. You should see the following:

<img src="../resources%20(ignore)/img/06/phone-1.png" width="250" height="444" />

### AddProductScreen.jsx

In the `screens` directory, create a new file called `AddProductScreen.jsx`. Add the following code:

```jsx
import { Button, View, Text, TextInput } from "react-native";
import { useState } from "react";
import { collection, addDoc } from "firebase/firestore";

import database from "../config/firebase"; // Get a Firestore instance

const AddProductScreen = (props) => {
  const [newProduct, setNewProduct] = useState({
    name: "",
    price: 0,
    createdAt: new Date(),
  });

  const onPress = async () => {
    // Add a new product to the Firestore database
    await addDoc(collection(database, "products"), newProduct);
    props.navigation.navigate("Home");
  };

  return (
    <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
      <Text>Add Product Screen</Text>
      <TextInput
        style={{
          width: 200,
          height: 40,
          borderColor: "gray",
          borderWidth: 1,
          marginBottom: 10,
        }}
        onChangeText={(name) => setNewProduct({ ...newProduct, name })}
        placeholder="Name"
      />
      <TextInput
        keyboardType="numeric"
        style={{ width: 200, height: 40, borderColor: "gray", borderWidth: 1 }}
        onChangeText={(price) =>
          setNewProduct({ ...newProduct, price: parseInt(price) })
        }
        placeholder="Price"
      />
      <Button title="Add Product" onPress={onPress} />
    </View>
  );
};

export default AddProductScreen;
```

Reload your application. You should see the following:

<img src="../resources%20(ignore)/img/06/phone-2.png" width="250" height="444" /> <img src="../resources%20(ignore)/img/06/phone-3.png" width="250" height="444" />

Press the "Add Product" button.

How do you know that the product was added to **Firestore database**? You can check **Firestore database** in the **Firebase console**:

![](<../resources%20(ignore)/img/06/web-17.png>)

Alternatively, you can add the following code to the `AddProductScreen.jsx` file:

```jsx
// ...
import { useEffect, useState } from "react";
import { collection, onSnapshot, orderBy, query } from "firebase/firestore";

import { database } from "../config/firebase";

const HomeScreen = (props) => {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    const fetchProducts = async () => {
      try {
        const q = query(
          collection(database, "products"),
          orderBy("createdAt", "desc")
        );

        const unsubscribe = onSnapshot(q, (querySnapshot) => {
          const productArr = [];
          querySnapshot.forEach((doc) => {
            productArr.push({
              id: doc.id,
              ...doc.data(),
            });
          });
          setProducts(productArr);
        });

        console.log(products);

        return () => unsubscribe(); // Detach listener
      } catch (err) {
        console.log(err);
      }
    };
    fetchProducts();
  }, []);

  // ...
};

// ...
```

Press J to open the debugger. Add another product. You should see the following:

![](<../resources%20(ignore)/img/06/web-18.png>)

**Note:** This code will be useful for the **research tasks**.

## Research Tasks

1. Display the products in a `FlatList` component. If no products are available, display a message that says "No products available".

2. Add a "Delete" button to each product. When the "Delete" button is pressed, delete the product from the **Firestore database**. **Note:** Use the `deleteDoc` function.

3. Add a "Update" button to each product. When the "Update" button is pressed, navigate to the `UpdateProductScreen`. The `UpdateProductScreen` should display the product's name and price. When the "Update" button is pressed, update the product in the **Firestore database**. **Note:** Use the `updateDoc` function.

## Submission

If you get stuck on any of the above research tasks, feel free to use **ChatGPT** permitting, you are aware of the following:

- If you provide **ChatGPT** with a prompt that is not refined enough, it may generate a not-so-useful response
- Do not trust **ChatGPT's** responses blindly. You must still use your judgement and may need to do additional research to determine if the response is correct
- Acknowledge that you are using **ChatGPT**. In the **README.md** file, please include what prompt(s) you provided to **ChatGPT** and how you used the response(s) to help you with your work

Create a new branch called **week-05-formative-assessment**. Add, commit and push your code.
