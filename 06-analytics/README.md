## Lab 6 - Analytics

---

To add analytics, we can use the following command:

```sh
$ amplify add analytics
```

> Next, we'll be prompted for the following:

- Provide your pinpoint resource name: __amplifyanalytics__   
- Apps need authorization to send analytics events. Do you want to allow guest/unauthenticated users to send analytics events (recommended when getting started)? __Y__   

To deploy, run the `push` command:

```sh
$ amplify push
```

### Recording events

Now that the service has been created we can now begin recording events.

To record analytics events, we need to import the `Analytics` class from Amplify & then call `Analytics.record`:

```js
import { Analytics } from 'aws-amplify'

state = {username: ''}

async componentDidMount() {
  try {
    const user = await Auth.currentAuthenticatedUser()
    this.setState({ username: user.username })
  } catch (err) {
    console.log('error getting user: ', err)
  }
}

recordEvent = () => {
  Analytics.record({
    name: 'My test event',
    attributes: {
      username: this.state.username
    }
  })
}

<Button onPress={this.recordEvent} title='Record Event' />
```

To view the analytics in the console, run the `console` command:

```sh
$ amplify console
```

In the console, click on __Analytics__, then click on __View  in Pinpoint__. In the __Pinpoint__ console, click on __events__ and then enable filters.

---

## Let's move to Lab 7!
### [Lab 7 - Adding Storage with Amazon S3](../07-storage/README.md)