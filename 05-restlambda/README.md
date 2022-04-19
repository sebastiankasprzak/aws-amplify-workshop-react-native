## Lab 5 - REST API with a Lambda Function

---

### Adding an API Endpoint

Now that we've created the cryptocurrency Lambda function let's add an API endpoint so we can invoke it via http.

To add the REST API, we can use the following command:

```sh
$ amplify add api

? Please select from one of the above mentioned services: REST
? Provide a friendly name for your resource that will be used to label this category in the project: cryptoapi   
? Provide a path (e.g., /items): /coins   
? Choose lambda source: Use a Lambda function already added in the current Amplify project   
? Choose the Lambda function to invoke by this path: cryptofunction   
? Restrict API access: Y
? Who should have access? Authenticated users only
? What kind of access do you want for Authenticated users: read/create/update/delete
? Do you want to add another path? (y/N) N  
```

Now the resources have been created & configured & we can push them to our account: 

```bash
$ amplify push

? Are you sure you want to continue? Y
```

---

### Interacting with the new API

Now that the API is created we can start sending requests to it & interacting with it.

Let's request some data from the API:

```js
// App.js
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'
import { API } from 'aws-amplify'
import { withAuthenticator } from 'aws-amplify-react-native'

import Amplify from 'aws-amplify'
import config from './aws-exports'
Amplify.configure(config)

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

---

## Let's move to Lab 6!
### [Lab 6 - Analytics](../06-analytics/README.md)