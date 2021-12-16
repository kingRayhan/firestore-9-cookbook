# Firestore 9 cheatsheet

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
