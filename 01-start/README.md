## Lab 1 - Creating the initial App and connecting Amplify

---

### Getting Started

To get started, we first need to create a new React Native project & change into the new directory.

```bash
$ npx expo init RNAmplify

> Choose a template: blank

$ cd RNAmplify

$ npm install aws-amplify aws-amplify-react-native @react-native-community/netinfo @react-native-async-storage/async-storage @react-native-picker/picker
```

---

### Running the app

Next, run the app:

```sh
$ expo start
```

![Expo Start](expo-start.png)

You'll see a QR code within your terminal that you can scan with the Expo app on your iOS or Android device. Alternatively you can press ```a``` to launch the app within your Android Emulator or ```i``` to launch the app within your iOS Simulator.

__Quick Tip 1:__ Pressing ```d```  within the Expo terminal window will load a Developer Tools web portal which has additional options including allowing you to change from a local network to a tunnelled network connection if you're mobile device isn't on the same network as your local computer.

__Quick Tip 2:__ Pressing ```r``` within the Expo terminal window will force a hot reload. This can be handy if your app doesn't seem to have been updated on your device after you save a change in your IDE.

You should see the following when the app has been launched on the device:

![New project](new-app.png)

Leave expo running in that terminal window as React Native and Expo support hot reloading of the app as we make changes :) 

Open a second terminal window for commands we'll run with Amplify CLI.

---

### Initializing a new AWS Amplify Project

> Make sure to initialize this Amplify project in the root of your new React Native application

```bash
$ amplify init
```

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

Now, the AWS Amplify CLI has iniatilized a new project & you will see a couple of new files & folders: __amplify__ & __/src/aws-exports.js__. These files hold your project configuration.

---

### Configuring the React Native application

The next thing we need to do is to configure our React Native application to be aware of our new AWS Amplify project. We can do this by referencing the auto-generated __aws-exports.js__ file that is now in our root folder.

---

### Configure the app for Amplify

To configure the app, open __App.js__ and add the following code below the last import:

```js
// App.js
import Amplify from 'aws-amplify';
import config from './src/aws-exports';
Amplify.configure(config);
```

Now, our app is ready to start using our AWS services.

---

## Let's move to Lab 2!
### [Lab 2 - Authentication](../02-authentication/README.md)