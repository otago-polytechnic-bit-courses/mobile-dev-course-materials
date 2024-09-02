# 08: Camera, Audio and Video

## Getting Started

Create a new project using the following command:

```bash
npx create-expo-app 08-playground --template blank
```

## Camera

Camera enables you to take pictures and record video.

### Getting Started

To get started, install the following dependencies:

```bash
npm install expo-constants expo-camera expo-media-library @expo/vector-icons
```

- `expo-constants` - provides system information, i.e., device name, operating system, etc.
- `expo-camera` - provides access to the device's camera
- `expo-media-library` - provides access to the device's media library
- `@expo/vector-icons` - provides access to icons

### Button.jsx

```jsx
import { Text, TouchableOpacity, StyleSheet } from "react-native";
import { Entypo } from "@expo/vector-icons";

const Button = (props) => {
  return (
    <TouchableOpacity onPress={props.onPress} style={styles.button}>
      <Entypo name={props.icon} size={28} color="#f1f1f1" />
      <Text style={styles.text}>{props.title}</Text>
    </TouchableOpacity>
  );
};

const styles = StyleSheet.create({
  button: {
    height: 40,
    flexDirection: "row",
    alignItems: "center",
    justifyContent: "center",
  },
  text: {
    fontWeight: "bold",
    fontSize: 16,
    color: "#f1f1f1",
    marginLeft: 10,
  },
});

export default Button;
```

### App.jsx

```js
import { useState, useEffect, useRef } from "react";
import { Text, View, StyleSheet, Image } from "react-native";
import Constants from "expo-constants";
import { Camera } from "expo-camera";
import { createAssetAsync, requestPermissionsAsync } from "expo-media-library";

import Button from "./components/Button";

const App = () => {
  const [hasCameraPermission, setHasCameraPermission] = useState(null);
  const [image, setImage] = useState(null);

  const cameraRef = useRef(null);

  useEffect(() => {
    // Request permission to access camera. This will prompt the user for permission
    const requestCameraPermissions = async () => {
      try {
        await requestPermissionsAsync(); // Request permission to access media library
        const cameraStatus = await Camera.requestCameraPermissionsAsync(); // Request permission to access camera
        setHasCameraPermission(cameraStatus.status === "granted"); // Check if permission was granted
      } catch (err) {
        console.error(err);
      }
    };

    requestCameraPermissions();
  }, []);

  const takeAPicture = async () => {
    if (cameraRef) {
      try {
        const data = await cameraRef.current.takePictureAsync(); // Take a picture
        setImage(data.uri); // Set the image
      } catch (err) {
        console.log(err);
      }
    }
  };

  const savePicture = async () => {
    if (image) {
      try {
        await createAssetAsync(image); // Save the picture to the device's media library
        alert("Picture saved");
        setImage(null); // Reset the image
      } catch (err) {
        console.log(err);
      }
    }
  };

  return (
    <View style={styles.container}>
      {!image ? (
        <Camera style={styles.camera} ref={cameraRef} />
      ) : (
        <Image source={{ uri: image }} style={styles.camera} />
      )}

      <View style={styles.controls}>
        {image ? (
          <View
            style={{
              flexDirection: "row",
              justifyContent: "space-between",
              paddingHorizontal: 50,
            }}
          >
            <Button
              title="Retake"
              onPress={() => setImage(null)}
              icon="retweet"
            />
            <Button title="Save" onPress={savePicture} icon="check" />
          </View>
        ) : (
          <Button title="Take a Picture" onPress={takeAPicture} icon="camera" />
        )}
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    paddingTop: Constants.statusBarHeight,
    backgroundColor: "#000",
    padding: 8,
  },
  controls: {
    flex: 0.5,
  },
  camera: {
    flex: 5,
    borderRadius: 20,
  },
});

export default App;
```

Reload your application. You should see the following:

1. The user will be prompted to allow the application to access their media and photos

<img src="../resources%20(ignore)/img/08/phone-1.png" width="250" height="444" />

2. The user will be prompted to allow the application to take pictures and record video

<img src="../resources%20(ignore)/img/08/phone-2.png" width="250" height="444" />

3. Once the user allows permissions, they can take a picture

<img src="../resources%20(ignore)/img/08/phone-3.png" width="250" height="444" />

4. The user can then save the picture to their device or retake the picture

<img src="../resources%20(ignore)/img/08/phone-4.png" width="250" height="444" />

## Audio

Audio enables you to play internal and external audio, i.e., audio from your device or from the web. Audio can be in the form of a sound or music.

### Getting Started

To get started, install the following dependency:

```bash
npm install expo-av
```

- `expo-av` - provides a unified API for media playback

### App.jsx

```jsx
// ...
import { Audio } from "expo-av";

// ...

const App = () => {
  // ...
  const [sound, setSound] = useState(null);

  // ...

  useEffect(() => {
    return sound
      ? () => {
        sound.unloadAsync(); // Unload the sound
      }
      : undefined; // Do nothing
  }, [sound]);

  const playASound = async () => {
    // Load the sound from the assets directory
    const { sound } = await Audio.Sound.createAsync(require("./assets/sounds/music.mp3")
    );
    setSound(sound);
    await sound.playAsync(); // Play the sound
  }

  // ...

  return (
    <View style={styles.container}>
      {/* ... */}

      <View style={styles.controls}>
        {image ? (
          // ...
        ) : (
          <View
            style={{
              flexDirection: "row",
              justifyContent: "space-between",
              paddingHorizontal: 50,
            }}>
            <Button title="Take a Picture" onPress={takePicture} icon="camera" />
            <Button title="Play a Sound" onPress={playASound} icon="sound" />
          </View>
        )}
      </View>
    </View>
  );
};

// ...
```

Reload your application. You should see the following:

1. Clicking the "Play a Sound" button will play a sound

<img src="../resources%20(ignore)/img/08/phone-5.png" width="250" height="444" />

## Video

Video enables you to play internal and external videos, i.e., videos from your device or from the web.

### App.jsx

```jsx
import { useRef, useState } from "react";
import { View, StyleSheet, Button } from "react-native";
import { Video, ResizeMode } from "expo-av";

const App = () => {
  const video = useRef(null);
  const [status, setStatus] = useState({});

  return (
    <View style={styles.container}>
      <Video
        ref={video} // Reference to the video
        style={styles.video}
        source={{
          uri: "https://d23dyxeqlo5psv.cloudfront.net/big_buck_bunny.mp4",
        }}
        useNativeControls // Use the native controls, i.e., play, pause, etc.
        resizeMode={ResizeMode.CONTAIN} // Resize the video to fit the screen
        isLooping // Loop the video
        onPlaybackStatusUpdate={(status) => setStatus(() => status)} // Update the status of the video, i.e., isPlaying, isBuffering, etc.
      />
      <View style={styles.buttons}>
        <Button
          title={status.isPlaying ? "Pause" : "Play"}
          onPress={() =>
            status.isPlaying
              ? video.current.pauseAsync() // Pause the video
              : video.current.playAsync() // Play the video
          }
        />
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    backgroundColor: "#ecf0f1",
  },
  video: {
    alignSelf: "center",
    width: 320,
    height: 200,
  },
  buttons: {
    flexDirection: "row",
    justifyContent: "center",
    alignItems: "center",
  },
});

export default App;
```

Reload your application. You should see the following:

1. Clicking the "Play" button will play the video

<img src="../resources%20(ignore)/img/08/phone-6.png" width="250" height="444" />

2. Clicking the "Pause" button will pause the video

<img src="../resources%20(ignore)/img/08/phone-7.png" width="250" height="444" />

## Research Tasks

1. Display "No access to camera" in a `Text` component if the user has not given the application permission to access the camera.

2. In this task, you will integrate the functionality learned in `01-react-native-and-expo` with the camera code snippets above. When the user takes a photo, the user should be able to access the photo from their device's media library and display it appropriately. 

3. Download five different sounds from [Zapsplat](https://www.zapsplat.com/) and add them to your project. When the user clicks the "Play a Sound" button, a random sound should be played.

4. Download five different videos from [Pexels](https://www.pexels.com/videos/) and add them to your project. When the user clicks the "Play a Video" button, a random video should be played.

## Submission

If you get stuck on any of the above research tasks, feel free to use **ChatGPT** permitting, you are aware of the following:

- If you provide **ChatGPT** with a prompt that is not refined enough, it may generate a not-so-useful response
- Do not trust **ChatGPT's** responses blindly. You must still use your judgement and may need to do additional research to determine if the response is correct
- Acknowledge that you are using **ChatGPT**. In the **README.md** file, please include what prompt(s) you provided to **ChatGPT** and how you used the response(s) to help you with your work

Create a new branch called **week-08-formative-assessment**. Add, commit and push your code.
