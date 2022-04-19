## Lab 4 - Serverless Functions

---

### Adding a basic Lambda Function

To add a serverless function, we can run the following command:

```sh
$ amplify add function

? Select which capability you want to add: Lambda function (serverless function)
? Provide an AWS Lambda function nam: basiclambda
? Choose the runtime that you want to use: NodeJS
? Choose the function template that you want to use: Hello World
? Do you want to configure advanced settings? N
? Do you want to edit the local lambda function now? Y
```

> This should open the function package located at __amplify/backend/function/basiclambda/src/index.js__.

Edit the function to look like this, & then save the file.

```js
exports.handler = async (event, contex) => {
  console.log('event: ', event)
  const body = {
    message: "Hello world!"
  }
  const response = {
    statusCode: 200,
    body
  }
  return response
}
```

Next, we can test this out by running:

```sh
$ amplify mock function basiclambda

? Provide the path to the event JSON object: src/event.json
```

You'll notice the following output from your terminal:

```sh
Ensuring latest function changes are built...
Starting execution...
event:  { key1: 'value1', key2: 'value2', key3: 'value3' }
Result:
{
  "statusCode": 200,
  "body": {
    "message": "Hello world!"
  }
}
Finished execution.
```

_Where is the event data coming from? It is coming from the values located in event.json in the function folder (__amplify/backend/function/basiclambda/src/event.json__). If you update the values here, you can simulate data coming arguments the event._

Feel free to test out the function by updating `event.json` with data of your own.

---

### Adding a function running an express server and invoking it from an API call (http)

Next, we'll build a function that will be running an [Express](https://expressjs.com/) server inside of it.

This new function will fetch data from a cryptocurrency API & return the values in the response.

To get started, we'll create a new function:

```sh
$ amplify add function

? Select which capability you want to add: Lambda function (serverless function)
? Provide an AWS Lambda function name: cryptofunction
? Choose the runtime that you want to use: NodeJS
? Choose the function template that you want to use: Serverless ExpressJS
? Do you want to configure advanced settings? No
? Do you want to edit the local lambda function now? Y
```

This should open the function package located at __amplify/backend/function/cryptofunction/src/index.js__. You'll notice in this file, that the event is being proxied into an express server:

```js
exports.handler = (event, context) => {
  console.log(`EVENT: ${JSON.stringify(event)}`);
  awsServerlessExpress.proxy(server, event, context);
};
```

Instead of updating the handler function itself, we'll instead update __amplify/backend/function/cryptofunction/src/app.js__ which has the actual server code we would like to be working with.

Here, in __amplify/backend/function/cryptofunction/src/app.js__, we'll add the following code & save the file:

```js
// amplify/backend/function/cryptofunction/src/app.js

// you should see this code already there 👇:
app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*")
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept")
  next()
});
// below the above code, add the following code 👇 (be sure not to delete any other code from this file)
const axios = require('axios')

app.get('/coins', function(req, res) {
  let apiUrl = `https://api.coinlore.com/api/tickers?start=0&limit=10`
  
  if (req && req.query) {
    // here we are checking to see if there are any query parameters, and if so appending them to the request
    const { start = 0, limit = 10 } = req.query
    apiUrl = `https://api.coinlore.com/api/tickers/?start=${start}&limit=${limit}`
  }

  axios.get(apiUrl)
    .then(response => {
      res.json({
        coins: response.data.data
      })
    })
    .catch(err => res.json({ error: err }))
})
```

In the above function we've used the __axios__ library to call another API. In order to use __axios__, we need be sure that it will be installed by updating the __package.json__ for the new function:

```sh
$ cd amplify/backend/function/cryptofunction/src

$ npm install axios

$ cd ../../../../../
```

Next, change back into the root directory.

---

## Let's move to Lab 5!
### [Lab 5 - REST API with a Lambda Function](../05-restlambda/README.md)