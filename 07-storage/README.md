## Lab 7 - Adding Storage with Amazon S3

---

### Adding Storage
To add storage, we can use the following command:

```sh
amplify add storage
```

> Answer the following questions   

- Please select from one of the below mentioned services __Content (Images, audio, video, etc.)__
- Please provide a friendly name for your resource that will be used to label this category in the
 project: __rnworkshopstorage__
- Please provide bucket name: __YOUR_UNIQUE_BUCKET_NAME__
- Who should have access: __Auth users only__
- What kind of access do you want for Authenticated users?

```sh
❯◉ create/update
 ◉ read
 ◉ delete
```


```sh
amplify push
```

Now, storage is configured & ready to use.

What we've done above is created configured an Amazon S3 bucket that we can now start using for storing items.

For example, if we wanted to test it out we could store some text in a file like this:

```js
import { Storage } from 'aws-amplify'

// create function to work with Storage
addToStorage = () => {
  Storage.put('textfiles/mytext.txt', `Hello World`)
    .then (result => {
      console.log('result: ', result)
    })
    .catch(err => console.log('error: ', err));
}

// add click handler
<Button onPress={this.addToStorage} title='Add to Storage' />
```

This would create a folder called `textfiles` in our S3 bucket & store a file called __mytext.txt__ there with the code we specified in the second argument of `Storage.put`.

If we want to read everything from this folder, we can use `Storage.list`:

```js
readFromStorage = () => {
  Storage.list('textfiles/')
    .then(data => console.log('data from S3: ', data))
    .catch(err => console.log('error fetching from S3', err))
}
```

If we only want to read the single file, we can use `Storage.get`:

```js
readFromStorage = () => {
  Storage.get('textfiles/mytext.txt')
    .then(data => {
      console.log('data from S3: ', data)
      fetch(data)
        .then(r => r.text())
        .then(text => {
          console.log('text: ', text)
        })
        .catch(e => console.log('error fetching text: ', e))
    })
    .catch(err => console.log('error fetching from S3', err))
}
```

If we wanted to pull down everything, we can use `Storage.list`:

```js
readFromStorage = () => {
  Storage.list('')
    .then(data => console.log('data from S3: ', data))
    .catch(err => console.log('error fetching from S3', err))
}
```

---

## Let's move to Lab 8!
### [Lab 8 - Multiple Serverless Environments](../08-multienv/README.md)