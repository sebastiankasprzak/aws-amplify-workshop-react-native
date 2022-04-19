## Pre-Requisites

---

### You will need the following in order to execute this Workshop

An AWS Account with sufficient permission to create AWS IAM, Amazon DynamoDB, Amazon Cognito and AWS Amplify resources.

Some basic experience with React web development, or native iOS / Android app development.

An editor that is compatible with React Native. This Workshop provides prescriptive guidance for Visual Studio Code. 

---

### Installing XCode and Android Studio
XCode and Android Studio contains CLI commands that will be needed for React Native at certain points of your build and publish processes. Device emulator / simulators are packaged as part of these IDE's also which make testing easier on virtual devices.

* Follow the [iOS Expo guide](https://docs.expo.dev/workflow/ios-simulator/) to download and install XCode and create a iOS device simulator. This is required for iOS builds and publishes, and is available on Mac platform only.

* Follow the [Android Expo guide](https://docs.expo.dev/workflow/android-studio-emulator/) to download and install Android Studio and create a Android device emulator. Would recommend to use Android Emulator 31+.

Expo has a simplified way to test your application on physical devices without requiring OS specific IDE's. We will use this in the workshop to test on Android and iOS phyisical devices.

---

### Installing React Native and Expo
Install [NodeJS version 16.14.4](https://nodejs.org/en/download/) or higher (Node has NPM bundled as a package manager). 

[Install Expo](https://docs.expo.dev/get-started/installation/) - Within an terminal run ```npm install --global expo-cli```. The Expo CLI makes building and testing React Natives apps simpler for beginners to mobile application development.

[Install Watchman](https://facebook.github.io/watchman/docs/install#buildinstall) - To monitor dependancy changes for hot reloading. This is available via package managers including [Homebrew on MacOS](https://docs.brew.sh/Installation) and [Chocolatey on Windows](https://chocolatey.org/install). You may need to run the package manager installation with elevated privileges (sudo on Mac, or right clicking your terminal app and choosing 'Run as Adminstrator' on Windows)

Lastly, download the Expo app on your iOS or Android device if you'd like to test your app on a physical device.

* [iOS Appstore](https://apps.apple.com/us/app/expo-go/id982107779)
* [Google Play](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_AU&gl=US)

---

### Installing Amplify
[Install Amplify CLI](https://docs.amplify.aws/cli/start/install) - Within an terminal run ```npm install -g @aws-amplify/cli```. Amplify CLI is the tool we will use to create and modify the AWS Amplify resources.

Configure Amplify CLI. For further details you can [watch the video guide](https://docs.amplify.aws/cli/start/install#option-1-watch-the-video-guide) or follow [these instructions](https://docs.amplify.aws/cli/start/install#option-2-follow-the-instructions)

---

## Let's begin!
### [Lab 1 - Creating the initial App and connecting Amplify](../01-start/README.md)