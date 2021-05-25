# Building a React Native Chat App using Expo and Gifted Chat \(Part 1\)

After reading this article, you will be able to:

1. Create an Expo React Native application
2. Create a Firebase project for user authentication
3. Create a ChatKitty project and connect to ChatKitty to provide real-time chat functionality
4. Use Firebase Authentication and ChatKitty Chat Functions to securely implement user login

If you want to skip ahead, the next article in this series covers [**building a group chat screen**](blog/posts/building-a-chat-app-with-react-native-and-gifted-chat-part-2/).

**You can find the complete source code for this project inside** [**this GitHub repository**](https://github.com/ChatKitty/chatkitty-example-react-native/tree/tutorial/part-1)**.**

## What is React Native?

[React Native](https://reactnative.dev/) is a great way to develop both web and mobile applications very quickly while sharing a lot of code when targeting multiple platforms. With a mature ecosystem of libraries and tooling, using React Native is not only fast but also reliable. Trusted by organizations like Facebook, Shopify, and Tesla - React Native is a stable framework for building both iOS and Android apps.

## What is Expo?

The [Expo](https://expo.io/) framework builds on top of React Native to allow developers to build universal React applications in minutes. With Expo, you can develop, build, deploy and quickly iterate on iOS, Android and web apps from the same JavaScript code. Expo has made creating both web and mobile applications very accessible, handling would-be complex workflows like multi-platform deployment and advanced features like push notifications.

## What is Firebase?

[Firebase](https://firebase.google.com/) is a [Backend-as-a-Service](https://www.cloudflare.com/learning/serverless/glossary/backend-as-a-service-baas/) offering by Google. It provides developers with a wide array of tools and services to develop quality apps without having to manage servers. Firebase provides key features like authentication, a real-time database, and hosting.

## What are ChatKitty Chat Functions?

ChatKitty provides **Chat Functions**, [serverless](https://www.cloudflare.com/learning/serverless/what-is-serverless/) cloud functions that allow you to define custom logic for complex tasks like user authentication, and respond to chat events that happen within your application. With ChatKitty Chat Functions, there's no need for you to build a backend to develop chat apps. ChatKitty Chat Functions auto-scale for you, and only cost you when they run. Chat Functions lower the total cost of maintaining your chat app, enabling you to build more logic, faster.

## Prerequisites

To develop apps with Expo and React Native, you should be able to write and understand JavaScript or TypeScript code. To define ChatKitty Chat Functions, you'll need to be familiar with basic JavaScript.

You'll need a version of [Node.js](https://nodejs.org/en/download/) above `10.x.x` installed on your local machine to build this React Native app.

You'll need to install the [Expo CLI tool](https://docs.expo.io/workflow/expo-cli/) through npm or npx.

For a complete walk-through on how to set up a development environment for Expo, you can go through [the official documentation here](https://docs.expo.io/get-started/installation/).

You can check out our Expo React Native sample code at any time [on GitHub](https://github.com/ChatKitty/chatkitty-example-react-native/).

## Creating the project and installing libraries

First, initialize a new Expo project with the **blank managed** workflow. To do so, you're going to need to open a terminal window and execute:

```bash
expo init chatkitty-example-react-native
```

After creating the initial application. You can enter the app root directory and run the app:

```bash
# navigate inside the project directory
cd chatkitty-example-react-native

# for android
expo start --android

# for ios
expo start --ios

# for web 
expo start --web
```

If you run your newly created React Native application using Expo, you should see:

![Created Project](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-created-project.png)

Now we have our blank project, we can install the React Native libraries we'll need:

```bash
# install following libraries for React Native
expo install @react-navigation/native @react-navigation/stack react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view react-native-paper react-native-vector-icons
```

Next, let's install Firebase:

```bash
# install Firebase
yarn add firebase@7.24.0
```

## Creating reusable form elements

We'll be creating Login and Signup screens soon which share similar logic. To prevent us from violating the [DRY principle](https://thevaluable.dev/dry-principle-cost-benefit-example/), let's create some reusable form components that we can share across these two screens. We'll also create a loading spinner component to provide a good user experience whenever a user waits for a long screen transition.

We'll create reusable `FormInput`, `FormButton`, and `Loading` UI components. At the root of this Expo React Native app, create a `src/` directory and inside it create another new `components/` directory.

Inside the `src/components/` directory, create a new JavaScript file `FormInput.js`. In this file, we'll define a [React component](https://reactjs.org/docs/react-component.html) to provide a text input field for our Login and Signup screens to use for the user to enter their credentials.

The `FormInput.js` file should contain the following code snippet:

{% code title="FormInput.js" %}
```jsx
import React from 'react';
import { Dimensions, StyleSheet } from 'react-native';
import { TextInput } from 'react-native-paper';

const { width, height } = Dimensions.get('screen');

export default function FormInput({ labelName, ...rest }) {
  return (
      <TextInput
          label={labelName}
          style={styles.input}
          numberOfLines={1}
          {...rest}
      />
  );
}

const styles = StyleSheet.create({
  input: {
    marginTop: 10,
    marginBottom: 10,
    width: width / 1.5,
    height: height / 15,
  },
});
```
{% endcode %}

Our next reusable component is going to be in another file `FormButton.js`. We use it to display a button for a user to confirm their credentials.

The `FormButton.js` file should contain:

{% code title="FormButton.js" %}
```jsx
import React from 'react';
import { Dimensions, StyleSheet } from 'react-native';
import { Button } from 'react-native-paper';

const { width, height } = Dimensions.get('screen');

export default function FormButton({ title, modeValue, ...rest }) {
  return (
      <Button
          mode={modeValue}
          {...rest}
          style={styles.button}
          contentStyle={styles.buttonContainer}
      >
        {title}
      </Button>
  );
}

const styles = StyleSheet.create({
  button: {
    marginTop: 10,
  },
  buttonContainer: {
    width: width / 2,
    height: height / 15,
  },
});
```
{% endcode %}

Finally, create a `Loading.js` file. We'll use it to display a loading spinner when a user waits for a screen transition.

The `Loading.js` file should contain:

{% code title="Loading.js" %}
```jsx
import React from 'react';
import { ActivityIndicator, StyleSheet, View } from 'react-native';

export default function Loading() {
  return (
      <View style={styles.loadingContainer}>
        <ActivityIndicator size="large" color="#5b3a70" />
      </View>
  );
}

const styles = StyleSheet.create({
  loadingContainer: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```
{% endcode %}

Now we have our reusable form components, we can create a Login screen for users to enter our chat app.

## Creating a login screen

The first screen we'll be creating is the login screen. We'll ask an existing user for their email and password to authenticate and provide a link to a future sign-up form for new users to register with our app.   
  
 The login screen should look like this after you're done:

![TODO: Login screen](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-login-screen.png)

Inside the `src/`, create a `screens/` directory, inside this directory create a `LoginScreen.js` file.

The `LoginScreen.js` file should contain:

{% code title="LoginScreen.js" %}
```jsx
import React, { useState } from 'react';
import { StyleSheet, View } from 'react-native';
import { Title } from 'react-native-paper';

import FormButton from '../components/FormButton';
import FormInput from '../components/FormInput';

export default function LoginScreen({ navigation }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  return (
      <View style={styles.container}>
        <Title style={styles.titleText}>Welcome!</Title>
        <FormInput
            labelName="Email"
            value={email}
            autoCapitalize="none"
            onChangeText={(userEmail) => setEmail(userEmail)}
        />
        <FormInput
            labelName="Password"
            value={password}
            secureTextEntry={true}
            onChangeText={(userPassword) => setPassword(userPassword)}
        />
        <FormButton
            title="Login"
            modeValue="contained"
            labelStyle={styles.loginButtonLabel}
            onPress={() => {
              // TODO
            }}
        />
        <FormButton
            title="Sign up here"
            modeValue="text"
            uppercase={false}
            labelStyle={styles.navButtonText}
            onPress={() => navigation.navigate('Signup')}
        />
      </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#f5f5f5',
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  titleText: {
    fontSize: 24,
    marginBottom: 10,
  },
  loginButtonLabel: {
    fontSize: 22,
  },
  navButtonText: {
    fontSize: 16,
  },
});
```
{% endcode %}

Later, you'll hook up this login screen to ChatKitty to log in users into your app. We've also configured the `navigation` prop to navigate the user to the Signup screen you'll soon be creating. For now, let's add a stack navigator to direct users to the initial login route.

Create a new directory `src/navigation/`. This will contain all the routes and components needed to build the app's navigation. Inside this directory, create a `AuthStack.js` file.

The `AuthStack.js` file should contain:

{% code title="AuthStack.js" %}
```jsx
import { createStackNavigator } from '@react-navigation/stack';
import React from 'react';

import LoginScreen from '../screens/LoginScreen';

const Stack = createStackNavigator();

export default function AuthStack() {
  return (
      <Stack.Navigator initialRouteName="Login" headerMode="none">
        <Stack.Screen name="Login" component={LoginScreen} />
      </Stack.Navigator>
  );
}
```
{% endcode %}

Later, you'll be adding another route for the Signup screen to our navigator.

Next, you'll need a navigation container to hold the app's stacks, starting with the auth stack. Create a `Routes.js` file inside the `src/navigation/` directory.

The `Routes.js` file should contain:

{% code title="Routes.js" %}
```jsx
import { NavigationContainer } from '@react-navigation/native';
import React from 'react';

import AuthStack from './AuthStack';

export default function Routes() {
  return (
      <NavigationContainer>
        <AuthStack />
      </NavigationContainer>
  );
}
```
{% endcode %}

We'll also be wrapping the app's routes in a paper provider that provides a theme for all the app components.

Create an `index.js` inside `src/navigation/`. This file should contain:

{% code title="index.js" %}
```jsx
import React from 'react';
import { DefaultTheme, Provider as PaperProvider } from 'react-native-paper';

import Routes from './Routes';

export default function Providers() {
  return (
      <PaperProvider theme={theme}>
        <Routes />
      </PaperProvider>
  );
}

const theme = {
  ...DefaultTheme,
  roundness: 2,
  colors: {
    ...DefaultTheme.colors,
    primary: '#5b3a70',
    accent: '#50c878',
    background: '#f7f9fb',
  },
};
```
{% endcode %}

Next, let's update the `App.js` file in the project root directory to use our providers.

The `App.js` file should now contain:

{% code title="App.js" %}
```jsx
import React from 'react';

import Providers from './src/navigation';

export default function App() {
  return <Providers />;
}
```
{% endcode %}

Now, if you run the app, you should see the login screen:

![Login screen](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-login-screen.png)

## Creating a sign-up screen

You'll need a way for a new user to sign up for your chat app so let's build a sign-up screen. We'll ask a new user for their email with a new password, as well as a display name that will be shown to other users in the app.

The sign-up screen should look like this after you're done:

![TODO: Signup screen](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-sign-up-screen.png)

Inside the `src/screens/` directory, create a `SignupScreen.js` file to hold your Signup screen component.

The `SignupScreen.js` file should contain:

{% code title="SignupScreen.js" %}
```jsx
import React, { useContext, useState } from 'react';
import { StyleSheet, View } from 'react-native';
import { IconButton, Title } from 'react-native-paper';

import FormButton from '../components/FormButton';
import FormInput from '../components/FormInput';

export default function SignupScreen({ navigation }) {
  const [displayName, setDisplayName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  return (
      <View style={styles.container}>
        <Title style={styles.titleText}>Let's get started!</Title>
        <FormInput
            labelName="Display Name"
            value={displayName}
            autoCapitalize="none"
            onChangeText={(userDisplayName) => setDisplayName(userDisplayName)}
        />
        <FormInput
            labelName="Email"
            value={email}
            autoCapitalize="none"
            onChangeText={(userEmail) => setEmail(userEmail)}
        />
        <FormInput
            labelName="Password"
            value={password}
            secureTextEntry={true}
            onChangeText={(userPassword) => setPassword(userPassword)}
        />
        <FormButton
            title="Signup"
            modeValue="contained"
            labelStyle={styles.loginButtonLabel}
            onPress={() => {
              // TODO
            }}
        />
        <IconButton
            icon="keyboard-backspace"
            size={30}
            style={styles.navButton}
            color="#5b3a70"
            onPress={() => navigation.goBack()}
        />
      </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#f5f5f5',
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  titleText: {
    fontSize: 24,
    marginBottom: 10,
  },
  loginButtonLabel: {
    fontSize: 22,
  },
  navButtonText: {
    fontSize: 18,
  },
  navButton: {
    marginTop: 10,
  },
});
```
{% endcode %}

Later, you'll hook up this screen to Firebase and ChatKitty to create the new user profile and begin a new chat session. Now, like with the Login screen, let's add this screen to the auth stack navigator.

Edit the `AuthStack.js` file you created earlier in `src/navigation/` with a stack screen component for the sign up screen.

The `AuthStack.js` file should contain:

{% code title="AuthStack.js" %}
```jsx
import { createStackNavigator } from '@react-navigation/stack';
import React from 'react';

import LoginScreen from '../screens/LoginScreen';
import SignupScreen from '../screens/SignupScreen';

const Stack = createStackNavigator();

export default function AuthStack() {
  return (
      <Stack.Navigator initialRouteName="Login" headerMode="none">
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Signup" component={SignupScreen} />
      </Stack.Navigator>
  );
}
```
{% endcode %}

If you run the app and tap the "Sign up here" form button text on the login screen, you should see the sign-up screen:

![Signup screen](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-sign-up-screen.png)

With both screens we need for user authentication, let's create a home screen to redirect users after they authenticate.

## Creating a home screen

For now, let's define a placeholder home screen to redirect users after they log in or sign up. In future tutorials, we'll flesh out this screen with complete real-time chat functionality.

Inside the `src/screens/` directory, create a `HomeScreen.js` file.

The `HomeScreen.js` file should contain:

{% code title="HomeScreen.js" %}
```jsx
import React from 'react';
import { StyleSheet, View } from 'react-native';
import { Title } from 'react-native-paper';

import FormButton from '../components/FormButton';

export default function HomeScreen() {
  return (
      <View style={styles.container}>
        <Title>ChatKitty Example</Title>
        <FormButton modeValue="contained" title="Logout" />
      </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#f5f5f5',
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```
{% endcode %}

You'll need another navigation stack to handle screen routes after the user logs in.

Create a `HomeStack.js` file inside the `src/navigation` directory.

The `HomeStack.js` file should contain:

{% code title="HomeStack.js" %}
```jsx
import { createStackNavigator } from '@react-navigation/stack';
import React from 'react';

import HomeScreen from '../screens/HomeScreen';

const Stack = createStackNavigator();

export default function HomeStack() {
  return (
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
  );
}
```
{% endcode %}

We'll need an authentication provider to check if a user is authenticated.

Inside the `src/navigation` directory create a `AuthProvider.js` file.

The `AuthProvider.js` file should contain:

{% code title="AuthProvider.js" %}
```jsx
import React, { createContext, useState } from 'react';

export const AuthContext = createContext({});

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);

  return (
      <AuthContext.Provider
          value={{
            user,
            setUser,
            loading,
            setLoading,
            login: async (email, password) => {
              // TODO
            },
            register: async (displayName, email, password) => {
              // TODO
            },
            logout: async () => {
              // TODO
            },
          }}
      >
        {children}
      </AuthContext.Provider>
  );
};
```
{% endcode %}

Later, you'll be updating the authentication provider to login, register, and logout the user using Firebase and ChatKitty.

To get the authentication context inside the app components, you'll need to wrap the app routes with the authentication provider.

Edit the `src/navigation/index.js` file to wrap the app routes with the authentication provider.

The `index.js` file should now contain:

{% code title="index.js" %}
```jsx
import React from 'react';
import { DefaultTheme, Provider as PaperProvider } from 'react-native-paper';

import { AuthProvider } from './AuthProvider';
import Routes from './Routes';

export default function Providers() {
  return (
      <PaperProvider theme={theme}>
        <AuthProvider>
          <Routes />
        </AuthProvider>
      </PaperProvider>
  );
}

const theme = {
  ...DefaultTheme,
  roundness: 2,
  colors: {
    ...DefaultTheme.colors,
    primary: '#5b3a70',
    accent: '#50c878',
    background: '#f7f9fb',
  },
};
```
{% endcode %}

Now, we have screens for user authentication and a place to redirect users after they authenticate. Let's hook the app up to a Firebase and ChatKitty backend.

## Creating a Firebase project

You'll be using Firebase to manage user passwords and authentication, so you'll need a Firebase project. If you don't already have one, create a new project using the [Firebase console](https://console.firebase.google.com/).

From the Firebase console home, create a project:

![Firebase create project](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-create-project.png)

Fill out the details of your Firebase project:

![Firebase create project details](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-create-project-details.png)

When you're done, click the "Continue" button, you'll be redirected to a dashboard screen for your new Firebase project.

![Firebase create project complete](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-create-project-complete.png)

To support user email and password login, you'll need to enable the Firebase email sign-in method. From the Firebase console side menu, navigate to the Authentication section.

![Firebase side menu authentication](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-side-menu-authentication.png)

Go to the "Sign-in method" tab and enable the email sign-in provider.

![Firebase enable email password sign in method](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-enable-email-password-sign-in-method.png)

### Adding Firebase credentials to the app

From the [Firebase console](https://console.firebase.google.com/) side menu, go to your "Project settings".

![Firebase project settings](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-project-settings.png)

Go to the "Your apps" section and click the Web icon:

![Firebase add app](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-add-app.png)

Fill out the application details

![Firebase create web app register](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-create-web-app-register.png)

Then continue to the console settings page

![Firebase create web app complete](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-create-web-app-complete.png)

Now, you should be able to copy your Firebase web app config to add to your Expo React Native project. From the "Your apps" section, select the "Config" **Firebase SDK snippet** for your web app and copy it:

![Firebase web app config](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-web-app-config.png)

In your Expo React Native project, inside the `src/` directory, create a `firebase/` directory. Inside the `src/firebase` directory, create an `index.js` file to hold your Firebase web app credentials.

The `index.js` file should contain:

{% code title="index.js" %}
```javascript
import * as firebase from 'firebase';
import '@firebase/auth';
import '@firebase/firestore';

// Replace this with your Firebase SDK config snippet
const firebaseConfig = {
  /* YOUR FIREBASE CONFIG OBJECT HERE */
};

if (!firebase.apps.length) {
  firebase.initializeApp(firebaseConfig);
}

export { firebase };
```
{% endcode %}

_Replacing `firebaseConfig` with the value from your Firebase SDK config snippet._

That's it. You've now configured your Expo React Native app to use Firebase.

## Hooking up user registration to the Sign-up screen

You now have everything needed to create new users in Firebase. Let's partially implement the `register` function stub in the authentication provider to add new users to Firebase.

Edit the `src/navigation/AuthProvider.js` file to use Firebase to register new users.

The `AuthProvider.js` file should now contain:

{% code title="AuthProvider.js" %}
```jsx
import React, { createContext, useState } from 'react';

import { firebase } from '../firebase';

export const AuthContext = createContext({});

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);

  return (
      <AuthContext.Provider
          value={{
            user,
            setUser,
            loading,
            setLoading,
            login: async (email, password) => {
              // TODO
            },
            register: async (displayName, email, password) => {
              setLoading(true);

              try {
                await firebase
                .auth()
                .createUserWithEmailAndPassword(email, password)
                .then((credential) => {
                  credential.user
                  .updateProfile({ displayName: displayName })
                  .then(async () => {
                    // TODO start a user chat session and log the user in
                  });
                });
              } catch (e) {
                console.log(e);
              }

              setLoading(false);
            },
            logout: async () => {
              // TODO
            },
          }}
      >
        {children}
      </AuthContext.Provider>
  );
};
```
{% endcode %}

We can now hook the register function to the Signup screen.

Edit the `src/screens/SignupScreen.js` file to call the `register` function.

The `SignupScreen.js` file should now contain:

{% code title="SignupScreen.js" %}
```jsx
import React, { useContext, useState } from 'react';
import { StyleSheet, View } from 'react-native';
import { IconButton, Title } from 'react-native-paper';

import FormButton from '../components/FormButton';
import FormInput from '../components/FormInput';
import Loading from '../components/Loading';
import { AuthContext } from '../navigation/AuthProvider';

export default function SignupScreen({ navigation }) {
  const [displayName, setDisplayName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const { register, loading } = useContext(AuthContext);

  if (loading) {
    return <Loading />;
  }

  return (
      <View style={styles.container}>
        <Title style={styles.titleText}>Let's get started!</Title>
        <FormInput
            labelName="Display Name"
            value={displayName}
            autoCapitalize="none"
            onChangeText={(userDisplayName) => setDisplayName(userDisplayName)}
        />
        <FormInput
            labelName="Email"
            value={email}
            autoCapitalize="none"
            onChangeText={(userEmail) => setEmail(userEmail)}
        />
        <FormInput
            labelName="Password"
            value={password}
            secureTextEntry={true}
            onChangeText={(userPassword) => setPassword(userPassword)}
        />
        <FormButton
            title="Signup"
            modeValue="contained"
            labelStyle={styles.loginButtonLabel}
            onPress={() => register(displayName, email, password)}
        />
        <IconButton
            icon="keyboard-backspace"
            size={30}
            style={styles.navButton}
            color="#5b3a70"
            onPress={() => navigation.goBack()}
        />
      </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#f5f5f5',
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  titleText: {
    fontSize: 24,
    marginBottom: 10,
  },
  loginButtonLabel: {
    fontSize: 22,
  },
  navButtonText: {
    fontSize: 18,
  },
  navButton: {
    marginTop: 10,
  },
});
```
{% endcode %}

If you run the app now and submit a sign-up form

![Signup screen filled](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-sign-up-screen-filled.png)

A new Firebase user should be created. You can verify this by checking the Firebase dashboard, Under the "Authentication" "Users" section, you should see your user:

![Firebase created user](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-firebase-created-user.png)

To complete the sign-up and login process, you'll need a ChatKitty project to connect to the ChatKitty JavaScript SDK and create chat sessions for your users. Let's create a ChatKitty project now.

## Creating a ChatKitty project

You'll be using ChatKitty to provide real-time chat functionality for your chat app. You can [create a free ChatKitty account here](https://dashboard.chatkitty.com/authorization/register).

Once you've created a ChatKitty account, create an application for your Expo React Native project:

![ChatKitty create application](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-chatkitty-create-application.png)

### Adding Firebase to your Chat Runtime

Now, let's integrate Firebase into your ChatKitty project. ChatKitty makes it easy to integrate any back-end into a ChatKitty application using Chat Functions. Chat Functions let you write arbitrary code that runs any time a relevant event or action happens inside your app. You can send push notifications, create emails or make HTTP calls to your backend, as well as use a pre-initialized server-side ChatKitty SDK to make changes to your application in response to user actions. With ChatKitty, you can use any [NPM](https://www.npmjs.com/) package inside your Chat Functions as a **Chat Runtime** dependency.

Let's now add Firebase as a Chat Runtime dependency. From your ChatKitty application dashboard, go to the "Functions" page:

![ChatKitty side menu functions](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-chatkitty-side-menu-functions.png)

Go to the "Runtime" tab and add a new dependency to the [Firebase JavaScript SDK](https://www.npmjs.com/package/firebase) NPM package, `firebase`. Version `7.24.0` was the latest version as of the time this article was written.

![ChatKitty runtime add firebase](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-chatkitty-runtime-add-firebase.png)

 _Remember to click the "Save" icon to confirm your chat runtime dependencies changes._

Before you can use Firebase within your chat functions, the Firebase SDK needs to be initialized. You can add arbitrary code that runs before each chat function using a chat runtime **initialization script**.  
Let's add an initialization script to initialize the Firebase SDK.

From the "Runtime" tab, click the drop-down and select "Initialization Script". You import NPM module using the CommonJS `require` function. Import the Firebase NPM module and initialize Firebase using your Firebase SDK snippet config object:

```javascript
const firebase = require('firebase');

// Replace this with your Firebase SDK config snippet
const firebaseConfig = {
  /* YOUR FIREBASE CONFIG OBJECT HERE */
};

if (!firebase.apps.length) {
  firebase.initializeApp(firebaseConfig);
}
```

 _Replacing `firebaseConfig` with the value from your Firebase SDK config snippet._

Now we're ready to define a chat function to check if a user's email and password exists and matches what we expect from Firebase, whenever a user tries to begin a new chat session.

### Checking user credentials using a chat function

Before we let users begin a chat session and access sensitive data like messages, we need to be sure their credentials match what's stored in Firebase using a chat function.

From your ChatKitty application dashboard, go to the "Functions" page. The "User Attempted Start Session" event chat function should be selected:

![ChatKitty chat functions](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-chatkitty-chat-functions.png)

This chat function runs whenever a user attempts to start a chat session. Edit the chat function to **delegate** user authentication to Firebase.

```javascript
const firebase = require('firebase');

async function handleEvent(
  event: UserAttemptedStartSessionEvent,
  context: Context
) {
  const email = event.username; // React Native app sends the user's email as the username
  const password = event.authParams.password; // React Native app includes the user's password as an auth param

  const userApi = context.getUserApi(); // Preinitialized ChatKitty server-side SDK User API client

  const userCredential = await firebase
    .auth()
    .signInWithEmailAndPassword(email, password); // Check Firebase credentials

  // If a ChatKitty user doesn't yet exist for this user, create one
  await userApi.getUserExists(email).catch(async () => {
    await userApi.createUser({
      name: event.username,
      displayName: userCredential.user.displayName || event.username,
    });
  });
}
```

![ChatKitty chat function user attempted start session](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-chatkitty-chat-function-user-attempted-start-session.png)

 _Remember to click the "Save" icon to confirm your chat function changes._

Now, whenever a user tries to log in, ChatKitty checks if a user with their login credentials exists from your Firebase backend, and if so, begins a chat session. With that, we're ready to connect your Expo React Native app to the ChatKitty JavaScript Chat SDK.

## Connecting to ChatKitty JS SDK

To use the ChatKitty JavaScript Chat SDK, you'll need to add the [ChatKitty JavaScript SDK](https://www.npmjs.com/package/chatkitty) NPM package to your Expo React Native project:

```bash
yarn add chatkitty
```

Next, you'll need to configure the ChatKitty SDK with your ChatKitty API key. You can find your API key on the ChatKitty dashboard, inside the "Settings" page.

![ChatKitty side menu settings](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-chatkitty-side-menu-settings.png)

Copy the string value, under "API Key", you'll need it to initialize a ChatKitty client instance:

![ChatKitty settings API key](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-chatkitty-settings-api-key.png)

In your Expo React Native project, inside the `src/` directory, create a `chatkitty/` directory. Inside the `src/chatkitty` directory, create an `index.js` file to hold your ChatKitty client instance.

The `index.js` file should contain:

{% code title="index.js" %}
```javascript
import ChatKitty from 'chatkitty';

export const kitty = ChatKitty.getInstance('YOUR CHATKITTY API KEY HERE');
```
{% endcode %}

_With the API key from the ChatKitty dashboard_

Let's now complete the chat login flow with the initialized ChatKitty client instance.

## Completing user login and sign up with ChatKitty

You now have everything needed to implement a complete and secure chat app sign-up and login. Let's fill out the missing pieces left in our authentication provider.

Edit the `src/navigation/AuthProvider.js` file to use ChatKitty to log in users and create chat sessions.

The `AuthProvider.js` file should now contain:

{% code title="AuthProvider.js" %}
```jsx
import React, { createContext, useState } from 'react';

import { kitty } from '../chatkitty';
import { firebase } from '../firebase';

export const AuthContext = createContext({});

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);

  return (
      <AuthContext.Provider
          value={{
            user,
            setUser,
            loading,
            setLoading,
            login: async (email, password) => {
              setLoading(true);

              const result = await kitty.startSession({
                username: email,
                authParams: {
                  password: password,
                },
              });

              setLoading(false);

              if (result.failed) {
                console.log('Could not login');
              }
            },
            register: async (displayName, email, password) => {
              setLoading(true);

              try {
                await firebase
                .auth()
                .createUserWithEmailAndPassword(email, password)
                .then((credential) => {
                  credential.user
                  .updateProfile({ displayName: displayName })
                  .then(async () => {
                    const result = await kitty.startSession({
                      username: email,
                      authParams: {
                        password: password,
                      },
                    });

                    if (result.failed) {
                      console.log('Could not login');
                    }
                  });
                });
              } catch (e) {
                console.log(e);
              }

              setLoading(false);
            },
            logout: async () => {
              try {
                await kitty.endSession();
              } catch (e) {
                console.error(e);
              }
            },
          }}
      >
        {children}
      </AuthContext.Provider>
  );
};
```
{% endcode %}

Now, we can check if a user is logged in, and if so, route them to the home screen.

Edit the `src/navigation/Routes.js` file to change the current navigation stack depending on if a user is logged in or not.

The `Routes.js` file should now contain:

{% code title="Routes.js" %}
```jsx
import { NavigationContainer } from '@react-navigation/native';
import React, { useContext, useEffect, useState } from 'react';

import { kitty } from '../chatkitty';
import Loading from '../components/Loading';

import { AuthContext } from './AuthProvider';
import AuthStack from './AuthStack';
import HomeStack from './HomeStack';

export default function Routes() {
  const { user, setUser } = useContext(AuthContext);
  const [loading, setLoading] = useState(true);
  const [initializing, setInitializing] = useState(true);

  useEffect(() => {
    return kitty.onCurrentUserChanged((currentUser) => {
      setUser(currentUser);

      if (initializing) {
        setInitializing(false);
      }

      setLoading(false);
    });
  }, [initializing, setUser]);

  if (loading) {
    return <Loading />;
  }

  return (
      <NavigationContainer>
        {user ? <HomeStack /> : <AuthStack />}
      </NavigationContainer>
  );
}
```
{% endcode %}

Let's hook the login function to the Login screen.

Edit the `src/screens/LoginScreen.js` file to call the `login` function.

The `LoginScreen.js` file should now contain:

{% code title="LoginScreen.js" %}
```jsx
import React, { useContext, useState } from 'react';
import { StyleSheet, View } from 'react-native';
import { Title } from 'react-native-paper';

import FormButton from '../components/FormButton';
import FormInput from '../components/FormInput';
import Loading from '../components/Loading';
import { AuthContext } from '../navigation/AuthProvider';

export default function LoginScreen({ navigation }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const { login, loading } = useContext(AuthContext);

  if (loading) {
    return <Loading />;
  }

  return (
      <View style={styles.container}>
        <Title style={styles.titleText}>Welcome!</Title>
        <FormInput
            labelName="Email"
            value={email}
            autoCapitalize="none"
            onChangeText={(userEmail) => setEmail(userEmail)}
        />
        <FormInput
            labelName="Password"
            value={password}
            secureTextEntry={true}
            onChangeText={(userPassword) => setPassword(userPassword)}
        />
        <FormButton
            title="Login"
            modeValue="contained"
            labelStyle={styles.loginButtonLabel}
            onPress={() => login(email, password)}
        />
        <FormButton
            title="Sign up here"
            modeValue="text"
            uppercase={false}
            labelStyle={styles.navButtonText}
            onPress={() => navigation.navigate('Signup')}
        />
      </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#f5f5f5',
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  titleText: {
    fontSize: 24,
    marginBottom: 10,
  },
  loginButtonLabel: {
    fontSize: 22,
  },
  navButtonText: {
    fontSize: 16,
  },
});
```
{% endcode %}

Finally, hook the logout authentication function to the Home screen and show the user's display name.

Edit the `src/screens/HomeScreen.js` file to call the `logout` function and greet your user.

The `HomeScreen.js` file should now contain:

{% code title="HomeScreen.js" %}
```jsx
import React, { useContext } from 'react';
import { StyleSheet, View } from 'react-native';
import { Title } from 'react-native-paper';

import FormButton from '../components/FormButton';
import { AuthContext } from '../navigation/AuthProvider';

export default function HomeScreen() {
  const { user, logout } = useContext(AuthContext);

  return (
      <View style={styles.container}>
        <Title>Hello, {user.displayName}!</Title>
        <FormButton
            modeValue="contained"
            title="Logout"
            onPress={() => logout()}
        />
      </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: '#f5f5f5',
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```
{% endcode %}

If you run the application and try to log in with the test user credentials you created earlier

![Login screen filled](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-login-screen-filled.png)

You should be redirected to your home screen

![Home screen](https://www.chatkitty.com/images/blog/posts/building-a-chat-app-with-react-native-and-firebase-part-1/screenshot-home-screen.png)

_Awesome!_ You've completed the first part of this tutorial series and successfully implemented secure user authentication for your Expo React Native chat app, using Firebase and powered by the ChatKitty chat platform. By defining your authentication logic in a chat function while delegating user credentials checking to Firebase, you were able to extend the ChatKitty login flow to handle your specific case, without having to build your own backend.

