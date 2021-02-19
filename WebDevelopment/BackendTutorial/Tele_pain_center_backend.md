## Basic node.js, React, Redux setup

[ref](https://medium.com/@ypushpraj/starting-up-with-a-react-redux-node-application-part-i-cc078d3a880)

[ref2](https://medium.com/@ypushpraj/starting-up-with-a-react-redux-node-application-part-ii-688709870f66)

+ `npm init -y`
  `npm i express`

+ Basic `server.js` setup

  ```javascript
  const express = require("express");
  const bodyParser = require("body-parser");
  
  const app = express();
  const port = process.env.PORT || 5000;
  
  app.use(bodyParser.json());
  app.get("/api/hello", (req, res) => {
    res.send({ express: "Hello From Express" });
  });
  
  app.post("/api/data", (req, res) => {
    console.log(req.body);
    res.send(
      `I received your POST request. This is what you sent me: ${req.body.post}`
    );
  });SetupSetup
  
  app.listen(port, () => console.log(`Listening on port ${port}`));
  
  ```

+ In client `package.json` add: `"proxy": "http://localhost:5000/"`

+ Basic front end setup:

  ```javascript
    const [response, setResponse] = useState("");
    const [post, setPost] = useState("");
    const [responseToPost, setResponseToPost] = useState("");
  
    const callApi = async () => {
      const response = await fetch("/api/hello");
      const body = await response.json();
      if (response.status !== 200) throw Error(body.message);
      return body;
    };
  
    useEffect(() => {
      callApi()
        .then((res) => setResponse(res.express))
        .catch((err) => console.log(err));
      return () => {};
    }, []);
  
    const handleSubmit = async (e) => {
      e.preventDefault();
      const response = await fetch("/api/data", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ post }),
      });
      console.log("response");
      const body = await response.text();
      setResponseToPost(body);
    };
  ```

  

+ Redux Thunk:
  Redux Thunk middleware allows you to write action creators that return a function instead of an action. The thunk can be used to delay the dispatch of an action, or to dispatch only if a certain condition is met. The inner function receives the store methods dispatch and getState as parameters.

+ Redux action setup:

  ```javascript
  export const sendData = (url, payload) => {
    console.log({ payload });
    return (dispatch) => {
      return fetch(url, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(payload),
      })
        .then((res) => {
          return res.json();
        })
        .then((res) => {
          dispatch({ type: "DATA_FROM_BACKEND", dataFromBackend: res });
        })
        .catch((err) => {
          console.log(`API failed with error: ${err.message}`);
        });
    };
  };
  
  export const getData = (url) => {
    return (dispatch) => {
      return fetch(url)
        .then((res) => {
          return res.json();
        })
        .then((res) => {
          dispatch({ type: "GET_DATA", getDataFromBackend: res });
        })
        .catch((err) => {
          console.log("API failed");
        });
    };
  };
  
  ```

  



## Node.js connection to Firebase

[Ref](https://www.youtube.com/watch?v=Z87OZtIYC_0&t=145s) 

+ `npm install firebase-admin`

+ Basic setup

  ```javascript
  const admin = require("firebase-admin");
  require("dotenv").config();
  const serviceAccount = require(`${process.env.GOOGLE_APPLICATION_CREDENTIALS}`);
  admin.initializeApp({
    credential: admin.credential.cert(serviceAccount),
  });
  const db = admin.firestore();
  
  const data = {
    title: "sample title",
    text: "sample text",
  };
  
  db.collection("sampleData")
    .doc("sample")
    .set(data)
    .then(() => {
      console.log("new data written to database");
    });
  
  ```

  





### Best practices for managing credentials

[Ref](https://cloud.google.com/docs/authentication/production)

Credentials provide access to sensitive data. The following practices help protect access to your credentials.

- Do not embed secrets related to authentication in source code, such as [API keys](https://cloud.google.com/docs/authentication/api-keys), OAuth tokens, and service account keys. You can use an environment variable pointing to credentials outside of the application's source code, such as [Cloud Key Management Service](https://cloud.google.com/kms).
- Create and use different credentials for different contexts, such as in testing and production environments.
- Only transfer credentials over a secure channel such as HTTPS to prevent a third party from intercepting your credentials. Never transfer in clear text or as part of the URL.
- Never embed long-lived credentials into your client-side application. For example, do not embed service account keys into a mobile app. Client-side apps can be examined and credentials can easily be found and used by a third party.
- [Revoke a token](https://developers.google.com/identity/protocols/OAuth2WebServer#tokenrevoke) if you no longer need it.