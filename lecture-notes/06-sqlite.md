# 06: SQLite

## Getting Started

Create a new project using the following command:

```bash
npx create-expo-app 06-playground --template blank
```

## SQLite

SQLite is a software library that provides a relational database management system. The lite in SQLite means light weight in terms of setup, database administration, and required resources. SQLite does not require a separate server process or system to operate. SQLite is a popular choice as embedded database software for local/client storage in application software such as web browsers. It is arguably the most widely deployed database engine, as it is used today by several widespread browsers, operating systems, and embedded systems (such as mobile phones), among others.

## Expo SQLite

Expo provides a set of APIs that allow you to interact with a SQLite database. The `expo-sqlite` module provides a WebSQL-compliant database that is stored in the device's memory. The database is persisted across restarts of your app.

## Installation

To install the `expo-sqlite` module, run the following command:

```bash
npm install expo-sqlite@13.4.0
```

## App.jsx

Here is an example of how to use the `expo-sqlite` module:

```javascript
import { useEffect, useState } from "react";
import { Text, View } from "react-native";
import * as SQLite from "expo-sqlite";

// Open a database
const db = SQLite.openDatabase("myDatabase.db");

const App = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    // Create table if it doesn't exist
    db.transaction((tx) => {
      tx.executeSql(
        "CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY NOT NULL, name TEXT, age INTEGER);"
      );
    });

    // Check if the table is empty before inserting
    db.transaction(
      (tx) => {
        tx.executeSql(
          "INSERT INTO users (name, age) values (?, ?)",
          ["Alice", 25],
          () => console.log("Insert success"),
          (tx, error) => {
            console.error("Insert error:", error);
            return true; // Return true if you want to roll back the transaction
          }
        );
      },
      (err) => console.error("Transaction error:", err),
      () => console.log("Transaction successful")
    );

    // Fetch data from the table
    db.transaction((tx) => {
      tx.executeSql("SELECT * FROM users", [], (_, { rows }) => {
        setUsers(rows._array);
      });
    });
  }, []);

  return (
    <View>
      {users.map((user) => (
        <Text key={user.id}>
          {user.name}, {user.age}
        </Text>
      ))}
    </View>
  );
};

export default App;
```

Reload your application.

## Research Tasks

1. Display a list of users from the database in a `FlatList` component. Each user should be displayed as a `ListItem` component with the user's name and age.

2. Create a form that allows the user to add a new user to the database. The form should have two fields: name and age. When the user submits the form, the data should be inserted into the database.

3. Create a button that allows the user to delete all users from the database.

## Submission

If you get stuck on any of the above research tasks, feel free to use **ChatGPT** permitting, you are aware of the following:

- If you provide **ChatGPT** with a prompt that is not refined enough, it may generate a not-so-useful response
- Do not trust **ChatGPT's** responses blindly. You must still use your judgement and may need to do additional research to determine if the response is correct
- Acknowledge that you are using **ChatGPT**. In the **README.md** file, please include what prompt(s) you provided to **ChatGPT** and how you used the response(s) to help you with your work

Create a new branch called **week-05-formative-assessment**. Add, commit and push your code.