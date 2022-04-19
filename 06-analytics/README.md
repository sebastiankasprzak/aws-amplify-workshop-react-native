## Lab 6 - Analytics

---

### Adding Analytics
To add analytics, we can use the following command:

```sh
$ amplify add analytics
```

> Next, we'll be prompted for the following:

- Select an Analytics provider: __Amazon Pinpoint__
- Provide your pinpoint resource name: __amplifyanalytics__   
- Apps need authorization to send analytics events. Do you want to allow guest/unauthenticated users to send analytics events (recommended when getting started)? __Y__   

To deploy, run the `push` command:

```sh
$ amplify push
```

---

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

This is what complete example looks like, building on existing code from previous exercise

```js
import React from 'react'
import { View, Text, StyleSheet, Button } from 'react-native'
import { API } from 'aws-amplify'
import { withAuthenticator } from 'aws-amplify-react-native'
import { Analytics } from 'aws-amplify'
import { Auth } from 'aws-amplify'

import Amplify from 'aws-amplify'
import config from './aws-exports'

Amplify.configure(config)
var state = {username: ''}


class App extends React.Component {
  state = {
    coins: []
  }
  async componentDidMount() {
    try {
      // to get all coins, do not send in a query parameter
      // const data = await API.get('cryptoapi', '/coins')
      const data = await API.get('cryptoapi', '/coins?limit=5&start=100')
      console.log('data from Lambda REST API: ', data)
      this.setState({ coins: data.coins })
    } catch (err) {
      console.log('error fetching data..', err)
    }
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
    console.log('Event recorded')
  }
  
  

  render() {
    return (
      <View>
        {
          this.state.coins.map((c, i) => (
            <View key={i} style={styles.row}>
              <Text style={styles.name}>{c.name}</Text>
              <Text>{c.price_usd}</Text>
            </View>
          ))
        }
        <Button onPress={this.recordEvent} title='Record Event' />
      </View>
      
    )
  }
}



const styles = StyleSheet.create({
  row: { padding: 10 },
  name: { fontSize: 20, marginBottom: 4 },
})

export default withAuthenticator(App, { includeGreetings: true })
```

To view the analytics in the console, run the `console analytics` command:

```sh
$ amplify console analytics
```

In the console, click on __Analytics__, then click on __View  in Pinpoint__. In the __Pinpoint__ console, click on __events__ and then enable filters.

---

## Let's move to Lab 7!
### [Lab 7 - Adding Storage with Amazon S3](../07-storage/README.md)