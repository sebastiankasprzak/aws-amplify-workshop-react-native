## Lab 2 - Authentication

---

### Adding Authentication

To add authentication, we can use the following command:

```sh
$ amplify add auth
```

- Do you want to use default authentication and security configuration? __Default configuration__   
- How do you want users to be able to sign in when using your Cognito User Pool? __Username__ (keep default) 
- Do you want to configure advanced settings? __No__

Now, we'll run the push command and the cloud resources will be created in our AWS account.

```bash
$ amplify push
```

To view the AWS services any time after their creation, run the following command:

```sh
$ amplify console

> AWS console
```

---

### Using the withAuthenticator component

To add authentication, we'll go into __App.js__ and first import the `withAuthenticator` HOC (Higher Order Component) from `aws-amplify-react`:

```js
// App.js
import { withAuthenticator } from 'aws-amplify-react-native';
```

Next, we'll wrap our default export (the App component) with the `withAuthenticator` HOC:

```js
export default withAuthenticator(App);
```

Now, we can run the app and see that an Authentication flow has been added in front of our App component. This flow gives users the ability to sign up & sign in.

To refresh, you can use one of the following commands:

```sh
# iOS Simulator
CMD + d # Opens debug menu
CMD + r # Reloads the app

# Android Emulator
CTRL + m # Opens debug menu
rr # Reloads the app
```

---

### Accessing User Data

We can access the user's info now that they are signed in by calling `Auth.currentAuthenticatedUser()`.

```js
// App.js
import React from 'react';
import {
  SafeAreaView,
  StyleSheet,
  Text,
} from 'react-native';

import { withAuthenticator } from 'aws-amplify-react-native'

import { Auth } from 'aws-amplify' 

class App extends React.Component {
  async componentDidMount() {
    const user = await Auth.currentAuthenticatedUser()
    console.log('user:', user)
  }
  render() {
    return (
      <SafeAreaView style={styles.container}>
        <Text style={styles.title}>Hello World</Text>
      </SafeAreaView>
    )
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  },
  title: {
    fontSize: 28
  }
})

export default withAuthenticator(App, {
  includeGreetings: true
})
```

---

### Signing out with a custom Sign Out button

We can also sign the user out using the `Auth` class & calling `Auth.signOut()`. This function returns a promise that is fulfilled after the user session has been ended & AsyncStorage is updated.

Because `withAuthenticator` holds all of the state within the actual component, we must have a way to rerender the actual `withAuthenticator` component by forcing React to rerender the parent component.

To do so, let's make a few updates:

```js
// App.js
import React from 'react';
import {
  SafeAreaView,
  StyleSheet,
  Text,
} from 'react-native';

import { withAuthenticator } from 'aws-amplify-react-native'

import { Auth } from 'aws-amplify' 

class App extends React.Component {
  async componentDidMount() {
    const user = await Auth.currentAuthenticatedUser()
    console.log('user:', user)
  }
  signOut = () => {
    Auth.signOut()
      .then(() => this.props.onStateChange('signedOut'))
      .catch(err => console.log('err: ', err))
  }
  render() {
    return (
      <SafeAreaView style={styles.container}>
        <Text style={styles.title}>Hello World</Text>
        <Text onPress={this.signOut}>Sign Out</Text>
      </SafeAreaView>
    )
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  },
  title: {
    fontSize: 28
  }
})

export default withAuthenticator(App);

```

---

### Custom authentication strategies

To view a final solution for a custom authentication strategy, check out the __AWS Amplify React Native Auth Starter__ [here](https://github.com/aws-samples/aws-amplify-auth-starters/tree/react-native#aws-amplify-react-native-auth-starter).

> This section is an overview and is considered an advanced part of the workshop. If you are not comfortable writing a custom authentication flow, I would read through this section and use it as a reference in the future. If you'd like to jump to the next section, click [here](https://github.com/MattJColes/aws-amplify-workshop-react-native#adding-a-graphql-api-with-aws-appsync).

The `withAuthenticator` component is a really easy way to get up and running with authentication, but in a real-world application we probably want more control over how our form looks & functions.

Let's look at how we might create our own authentication flow.

To get started, we would probably want to create input fields that would hold user input data in the state. For instance when signing up a new user, we would probably need 3 user inputs to capture the user's username, email, & password.

To do this, we could create some initial state for these values & create an event handler that we could attach to the form inputs:

```js
// initial state
state = {
  username: '', password: '', email: ''
}

// event handler
onChangeText = (key, value) => {
  this.setState({ [key]: value })
}

// example of usage with TextInput
<TextInput
  placeholder='username'
  value={this.state.username}
  style={{ width: 300, height: 50, margin: 5, backgroundColor: "#ddd" }}
  onChangeText={v => this.onChange('username', v)}
/>
```

We'd also need to have a method that signed up & signed in users. We can us the Auth class to do thi. The Auth class has over 30 methods including things like `signUp`, `signIn`, `confirmSignUp`, `confirmSignIn`, & `forgotPassword`. Thes functions return a promise so they need to be handled asynchronously.

```js
// import the Auth component
import { Auth } from 'aws-amplify'

// Class method to sign up a user
signUp = async() => {
  const { username, password, email } = this.state
  try {
    await Auth.signUp({ username, password, attributes: { email }})
  } catch (err) {
    console.log('error signing up user...', err)
  }
}
```

---

## Let's move to Lab 3!
### [Lab 3 - GraphQL API with AWS AppSync](../03-appsync/README.md)