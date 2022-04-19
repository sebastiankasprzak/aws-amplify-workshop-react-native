## Lab 1 - Creating the initial App and connecting Amplify

---

### Getting Started

To get started, we first need to create a new React Native project & change into the new directory.

```bash
$ npx expo init RNAmplify

> Choose a template: blank

$ cd RNAmplify

$ npm install --save aws-amplify aws-amplify-react-native uuid @react-native-community/netinfo

# or

$ npm add aws-amplify aws-amplify-react-native uuid
```

---

### Running the app

Next, run the app:

```sh
$ expo start
```
---

### Initializing A New AWS Amplify Project

> Make sure to initialize this Amplify project in the root of your new React Native application

```bash
$ amplify init
```

? Enter a name for the project __RNAmplify__

? Initialize the project with the above configuration? __No__

Enter following configuration:
- Enter a name for the project: __RNAmplify__
- Enter a name for the environment: __dev__
- Choose your default editor: __Visual Studio Code (or your favorite editor)__   
- Please choose the type of app that you're building __javascript__   
- What javascript framework are you using __react-native__   
- Source Directory Path: __/__   
- Distribution Directory Path: __/__
- Build Command: __npm run-script build__   
- Start Command: __npm run-script start__   
- Select the authentication method you want to use: __AWS profile__
- Please choose the profile you want to use: __amplify-workshop-user__

Now, the AWS Amplify CLI has iniatilized a new project & you will see a couple of new files & folders: __amplify__ & __aws-exports.js__. These files hold your project configuration.

---

### Configuring the React Native application

The next thing we need to do is to configure our React Native application to be aware of our new AWS Amplify project. We can do this by referencing the auto-generated __aws-exports.js__ file that is now in our root folder.

---

### Configure the app for Amplify

To configure the app, open __App.js__ and add the following code below the last import:

```js
// App.js
import Amplify from 'aws-amplify'
import config from './aws-exports'
Amplify.configure(config)
```

Now, our app is ready to start using our AWS services.

---

---

## Let's move to Lab 2!
### [Lab 2 - Authentication](../02-authentication/README.md)