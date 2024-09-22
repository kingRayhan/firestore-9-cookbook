# Firestore 9 cookbook


#### Table of contents
- [Initialization](#initialization)
- [Read doc/docs](#read-doc-docs)
  * [Read collection](#read-collection)
  * [Read a single document](#read-a-single-document)
  * [Check a document exists or not](#check-a-document-exists-or-not)
  * [Conditionally querying data](#conditionally-querying-data)
- [Store data](#store-data)
  * [Store a doc with auto generated id](#store-a-doc-with-auto-generated-id)
  * [Store a doc with custom id](#store-a-doc-with-custom-id)
- [Deleting document](#deleting-document)
  * [Delete a document with id](#delete-a-document-with-id)
- [Update document](#update-document)
- [Sorting/limiting](#sorting-limiting)
  * [Get limited number of docs](#get-limited-number-of-docs)
  * [Limit with order](#limit-with-order)





### Initialization

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";

const config = {
  apiKey: "xxxx",
  authDomain: "xxxx",
  projectId: "xxxx",
  storageBucket: "xxxx",
  messagingSenderId: "xxxx",
  appId: "xxxx",
};


export const app = initializeApp(config);
export const db = getFirestore(app);
```


### Read doc/docs

#### Read collection
```js
const collectionRef = collection(db, "users");

getDocs(collectionRef).then((snapshot) => {
  snapshot.docs.forEach((doc) => {
    console.log(doc.data());
  });
});
```

#### Read a single document
```js
const docRef = doc(db, "users", "G3OIFQ7qes9Rhc74XfRA");
// - OR
// const docRef = doc(db, "users/G3OIFQ7qes9Rhc74XfRA");

getDoc(docRef).then((snapshot) => {
  console.log(snapshot.data());
});
```

#### Check a document exists or not
```js
const docSnap = await getDoc(docRef);

if(!docSnap.exists()) {
  console.log("Document does not exist!");
}
```


#### Conditionally querying data
```js
const collectionRef = collection(db, "users");
const q = query(collectionRef, where("username", "==", "kingrayhan"));

const snapshot = await getDocs(q);
console.log(snapshot.docs[0].data());





// -- More query example
const stateQuery = query(citiesRef, where("state", "==", "CA"));
const populationQuery = query(citiesRef, where("population", "<", 100000));
const nameQuery = query(citiesRef, where("name", ">=", "San Francisco"));
```
**Query operator**
- `<` - less than
- `<=` - less than or equal to
- `==` - equal to
- `>` - greater than
- `>=` - greater than or equal to
- `!=` - not equal to
- `array-contains`
- `array-contains-any`
- `in`
- `not-in`



### Store data

#### Store a doc with auto generated id

```js
const collectionRef = collection(db, "users");
const docRef = await addDoc(collectionRef, {
  username: "johndoe",
  avatar: "https://avatars0.githubusercontent.com/u/174825?v=4",
});

console.log(docRef.id);
```

#### Store a doc with custom id

```js
const docRef = doc(db, "users", "user-id-custom");
// - OR
// const docRef = doc(db, "users/G3OIFQ7qes9Rhc74XfRA");

await setDoc(docRef, {
  username: "kingrayhan",
  avatar: "https://avatars0.githubusercontent.com/u/174825?v=4",
});
```

### Deleting document

#### Delete a document with id
```js
const docRef = doc(db, "users", "xj7lxm0OGObV91xn3tE0");
await deleteDoc(docRef);
```

### Update document
```js
const docRef = doc(db, "users", "AvPfqaJs4hCmqpk0RUjU");
// - OR
// const docRef = doc(db, "users/G3OIFQ7qes9Rhc74XfRA");


/**
 * Update document
 */
await updateDoc(docRef, {
  name: "Rayhan",
  username: "rayhan",
});

/**
 * Override the whole document
 */
await setDoc(docRef, {
  x: 10,
});
```

### Sorting/limiting

#### Get limited number of docs
```js
const colRef = collection(db, "posts");

const q = query(colRef, limit(5))
const postsSnap = await getDocs(q)

const posts = postsSnap.docs.map((post) => post.data())

// -> console.log(posts)
```

#### Limit with order
```js
const colRef = collection(db, "posts");

const q = query(colRef, limit(5), orderBy("createdAt", "desc"))
const postsSnap = await getDocs(q)

const posts = postsSnap.docs.map((post) => post.data())

// -> console.log(posts)
```
