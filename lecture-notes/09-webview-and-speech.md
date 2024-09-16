# 08: WebView and Speech

## Getting Started

Create a new project using the following command:

```bash
npx create-expo-app 09-playground --template blank
```

## WebView

A `WebView` enables you to display web content in your application. It is similar to an `iframe` in **HTML**.

### Getting Started

To get started, install the following dependency:

```bash
npm install react-native-webview@11.26.1
```

### App.jsx

```jsx
import { WebView } from "react-native-webview";
import { StyleSheet } from "react-native";
import Constants from "expo-constants";

const App = () => {
  return (
    <WebView
      style={styles.container}
      source={{ uri: "https://www.youtube.com/live/IDDmrzzB14M" }}
    />
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    marginTop: Constants.statusBarHeight,
  },
});

export default App;
```

Reload your application. You should see the following:

<img src="../resources%20(ignore)/img/09/phone-1.png" width="250" height="444" />

## Speech

### Getting Started

To get started, install the following dependency:

```bash
npm install expo-speech
```

### App.jsx

```jsx
import { View, StyleSheet, Button } from "react-native";
import { speak } from "expo-speech";

const App = () => {
  const speakText = () => {
    const thingToSay = "Hello, World!";
    speak(thingToSay);
  };

  return (
    <View style={styles.container}>
      <Button title="Press to hear some words" onPress={speakText} />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
  },
});

export default App;
```

## Research Tasks

1. What is the difference between a `WebView` and an `iframe`? What are the advantages and disadvantages of each? When would you use one over the other? Write your answers in the `09-research.md` file.

2. Display ten random YouTube video links in a `FlatList`. When the user clicks on a link, the video should play in a `WebView`.

3. Using the **Musixmatch** API, display the name of a song in a `Text` component. When the user clicks on the `Button` component, the lyrics should be read aloud using the `speak` function. 

Here is an example endpoint - <https://api.musixmatch.com/ws/1.1/track.get?commontrack_id=5920049&apikey=<ADD YOUR API KEY HERE>.

You can find a **Musixmatch** API key on the **Microsoft Teams** course channel under `Files` > `api-keys` > `musixmatch-api-key.txt`.

4. Using the **Yandex Translate** API, create a language translator. Create two `TextInput` components. When the user enters text into the first `TextInput` component, the text should be translated into the language specified in the second `TextInput` component. For example, if the user enters `Hello` into the first `TextInput` component and `fr` into the second `TextInput` component, the text should be translated into French.


Here is an example endpoint - <https://translate.yandex.net/api/v1.5/tr.json/translate?key=<ADD YOUR API KEY HERE>&text=hello&lang=en-fr>

You can find a **Yandex Translate** API key on the **Microsoft Teams** course channel under `Files` > `api-keys` > `yandex-api-key.txt`.

## Submission

If you get stuck on any of the above research tasks, feel free to use **ChatGPT** permitting, you are aware of the following:

- If you provide **ChatGPT** with a prompt that is not refined enough, it may generate a not-so-useful response
- Do not trust **ChatGPT's** responses blindly. You must still use your judgement and may need to do additional research to determine if the response is correct
- Acknowledge that you are using **ChatGPT**. In the **README.md** file, please include what prompt(s) you provided to **ChatGPT** and how you used the response(s) to help you with your work

Create a new branch called **week-08-formative-assessment**. Add, commit and push your code.
