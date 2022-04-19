## Lab 8 - Multiple Serverless Environments

---

Now that we have our API up & running, what if we wanted to update our API but wanted to test it out without it affecting our existing version?

To do so, we can create a clone of our existing environment, test it out, & then deploy & test the new resources.

Once we are happy with the new feature, we can then merge it back into our main environment. Let's see how to do this!

---

### Creating a new environment

To create a new environment, we can run the `env` command:

```sh
$ amplify env add

> Do you want to use an existing environment? No
> Enter a name for the environment: apiupdate
> Do you want to use an AWS profile? Yes
> Please choose the profile you want to use: appsync-workshop-profile
```

Now, the new environment has been initialize, & we can deploy the new environment using the `push` command:

```sh
$ amplify push
```

Now that the new environment has been created we can get a list of all available environments using the CLI:

```sh
$ amplify env list
```

Let's update the GraphQL schema to add a new field. In __amplify/backend/api/RestaurantAPI/schema.graphql__  update the schema to the following:

```graphql
type Restaurant @model {
  id: ID!
  clientId: String
  name: String!
  type: String
  description: String!
  city: String!
}

type ModelRestaurantConnection {
	items: [Restaurant]
	nextToken: String
}

type Query {
  listAllRestaurants(limit: Int, nextToken: String): ModelRestaurantConnection
}
```

In the schema we added a new field to the __Restaurant__ definition to define the type of restaurant:

```graphql
type: String
```

Now, we can run amplify push again to update the API:

```sh
$ amplify push
```

To test this out, we can go into the [AppSync Console](https://console.aws.amazon.com/appsync) & log into the API.

You should now see a new API called __RestaurantAPI-apiupdate__. Click on this API to view the API dashboard.

If you click on __Schema__ you should notice that it has been created with the new __type__ field. Let's try it out.

To test it out we need to create a new user because we are using a brand new authentication service. To do this, open the app & sign up.

In the API dashboard, click on __Queries__.

Next, click on the __Login with User Pools__ link.

Copy the __aws_user_pools_web_client_id__ value from your __aws-exports__ file & paste it into the __ClientId__ field.

Next, login using your __username__ & __password__.

Now, create a new mutation & then query for it:

```graphql
mutation createRestaurant {
  createRestaurant(input: {
    name: "Nobu"
    description: "Great Sushi"
    city: "New York"
    type: "sushi"
  }) {
    id name description city type
  }
}

query listRestaurants {
  listAllRestaurants {
    items {
      name
      description
      city
      type
    }
  }
}
```

---

### Merging the new environment changes into the main environment.

Now that we've created a new environment & tested it out, let's check out the main environment.

```sh
$ amplify env checkout local
```

Next, run the `status` command:

```sh
$ amplify status
```

You should now see an __Update__ operation:

```
Current Environment: local

| Category | Resource name   | Operation | Provider plugin   |
| -------- | --------------- | --------- | ----------------- |
| Api      | RestaurantAPI   | Update    | awscloudformation |
| Auth     | cognito75a8ccb4 | No Change | awscloudformation |
```

To deploy the changes, run the push command:

```sh
$ amplify push
```

Now, the changes have been deployed & we can delete the `apiupdate` environment:

```sh
$ amplify env remove apiupdate

Do you also want to remove all the resources of the environment from the cloud? Y
```

Now, we should be able to run the `list` command & see only our main environment:

```sh
$ amplify env list
```

---

## Removing Services

If at any time, or at the end of this workshop, you would like to delete a service from your project & your account, you can do this by running the `amplify remove` command:

```sh
$ amplify remove auth

$ amplify push
```

If you are unsure of what services you have enabled at any time, you can run the `amplify status` command:

```sh
$ amplify status
```

`amplify status` will give you the list of resources that are currently enabled in your app.

### Team environments

On top of multi-environment, Amplify has a number of best practises when operating within a Team of developers. Read more [here](https://docs.amplify.aws/cli/teams/overview/)

---

## Congratulations on finishing your React Native application! 
### [Let's clean up our cloud resources.](../99-cleanup/README.md)