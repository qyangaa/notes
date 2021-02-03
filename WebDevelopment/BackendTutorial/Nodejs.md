## Packages

+ cors: Cross-origin resource sharing (CORS) allows AJAX requests to skip the [Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) and access resources from remote hosts.
+ *mongoose*. It makes interacting with MongoDB through Node.js simpler.
+ *dotenv* loads environment variables from a `.env` file into `process.env`.
+ *Express*. a fast and lightweight web framework for Node.js.
+ *nodemon*: helps develop node.js based applications by automatically restarting the node application when file changes in the directory are detected.



### Mongoose

+ Schema

  ```javascript
  const Schema = mongoose.Schema;
  
  const userSchema = new Schema({
    username: {
      type: String,
      required: true,
      unique: true,
      trim: true,
      minlength: 3
    },
  }, {
    timestamps: true,
  });
  const User = mongoose.model("User", userSchema);
  ```

  

### Express

+ Router

```javascript
// server.js
const usersRouter = require("./routes/users");
app.use("/users", usersRouter);
```

```javascript
// ./routes.users.js
const router = require("express").Router();
let User = require("../models/user.model");

router.route("/").get((req, res) => {
  User.find()
    .then((users) => res.json(users))
    .catch((err) => res.status(400).json("Error: " + err));
});

router.route("/add").post((req, res) => {
  const username = req.body.username;
  const newUser = new User({ username });

  newUser
    .save()
    .then(() => res.json("User added!"))
    .catch((err) => res.status(400).json("Error: " + err));
});

module.exports = router;
```

