<h1 align="center">
    <img src=".github\logo.png" style="zoom:100%;" align="center"/>
</h1>
<p align="center">
    <img alt="License" src="https://img.shields.io/static/v1?label=License&message=MIT&color=FFFF00&labelColor=000000"><img alt="Version" src="https://img.shields.io/static/v1?label=Version&message=1.0&color=FFFF00&labelColor=000000"/>
</p>

# 💻 Project

# Create an Admin in a few minutes

https://adminbro.com

```md
We will play with AdminBro to create our CRUD
```

# 🛠 Environment

```md
We will need
```

1. Code Editor
2. Node.js
3. Mongo
4. Terminal

# 🚀 Start with adminbro

## Create structure

```sh
mkdir project
cd project
touch admin.js
```

## Install dependencies

```sh
npm i admin-bro @admin-bro/express express express-formidable
```

## admin.js

#### Admin Bro

```js
const AdminBro = require("admin-bro");
const AdminBroExpress = require("@admin-bro/express");

const adminBro = new AdminBro({
  databases: [],
  rootPath: "/admin",
});
const router = AdminBroExpress.buildRouter(adminBro);
```

#### Server

```js
const express = require("express");
const server = express();

server
  .use(adminBro.options.rootPath, router)
  .listen(5500, () => console.log("Server started"));
```

## Start Server

```sh
node admin.js
```

# 🗃 Add Database

## Install dependencies

```sh
npm i @admin-bro/mongoose mongoose
```

## update admin.js

#### Database

```js
const mongoose = require("mongoose");

const ProjectSchema = new mongoose.Schema({
  title: {
    type: String,
    require: true,
  },
  description: String,
  completed: Boolean,
  created_at: { type: Date, required: true, default: Date.now },
});

const Project = mongoose.model("Project", ProjectSchema);
```

#### AdminBro

```js
const AdminBro = require("admin-bro");
const AdminBroExpress = require("admin-bro/express");
const AdminBroMongoose = require("admin-bro/mongoose");
```

#### use mongoose in AdminBro

```js
AdminBro.registerAdapter(AdminBroMongoose);
```

#### config

```js
const adminBroOptions = new AdminBroOptions({
  resources: [Project],
  rootPath: "/admin",
});
const router = AdminBroExpress.buildRouter(adminBroOptions);
```

#### Server

```js
const express = require("express");
const server = express();

server.use(adminBroOptions.options.rootPath, router);
```

#### Run App

```js
const run = async () => {
  await mongoose.connect("mongodb://localhost/adminbroapp", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  });

  await server.listen(5500, () => console.log("Server started"));
};

run();
```

## Customize

https://adminbro.com/tutorial-customizing-resources.html

#### Change Project menu name and add options to resource

```js
const adminBroOptions = new AdminBro({
  resources: [
    {
      resource: Project,
      options: {
        properties: {
          description: { type: "richtext" },
          created_at: {
            isVisible: { edit: false, list: true, show: true, filter: true },
          },
        },
      },
    },
  ],
  locale: {
    translations: {
      labels: {
        Project: "My Projects",
      },
    },
  },
  rootPath: "/admin",
});
```

# 📄 License

```md
This project licensed under the MIT License. See the LICENSE file for details.
```
