# firestore-9-cheatsheet


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


