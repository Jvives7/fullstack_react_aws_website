# Overview

 # Table of Contents:
 - [Chapter 1 - Setup](#Chapter_1_setup)
 - [Chapter 2 - Initializing React App](#Chapter_2_init_react_app)
 - [Chapter 3 - Adding Authentication](#Chapter_3_add_auth)
 - [Chapter 4 - Adding GraphQL API and Database](#Chapter_4_add_api_db)
 - [Chapter 5 - Adding Storage and Associated Image](#Chapter_5)
# 
<!-- headings -->
<a id="Chapter_1_setup"></a>
# Chapter 1 - Setup


### Concepts
>amplify
###
> React
###

    react
### 
<!-- Add a Architecture Diagram -->

Topics covered in this part are:
1. Creating React app
2. Pushing react app to Github Repo
3. Setting up AWS Amplify to host Github Repo
4. Deploying Github Repo

### Steps 

1. Create a react app
```shell
npx create-react-app example
```
2. Change into example directory and initialize a git repository

```
cd example
git init
```


3. Create new github repository to put the react app into. 

4. Set origin. 

Copy ssh link and use the following command
```
git remote add origin <ssh clone link from repo>
git push origin master
```

4. Open AWS Amplify service and connect github to host react app

Reference/source: https://www.youtube.com/watch?v=DHLZAzdT44Y

<!-- headings -->
<a id="Chapter_2_init_react_app"></a>
# Chapter 2 - Initializing React App
Creating a simple web app using AWS Amplify

### Concepts
                Amplify Command Line Interface(CLI)
    Allows you to create. manage, and remove AWS services using terminal.
###
                Amplify Console 
    The AWS Amplify console is the control center for your full-stack app. Configure and manage your hosting and backend environments.

### 

### Steps

1. Install amplify CLI

a. Open a new terminal and enter following code. I had to used sudo.
```shell
npm install -g @aws-amplify/cli
```
reference: https://www.npmjs.com/package/@aws-amplify/cli

2. Configure Amplify CLI 

a. Run
```shell
amplify configure
```
b. Choose AWS region nearest to you and create a user name for new IAM user
![Alt text](<Screenshot 2023-11-25 at 6.32.15 AM.png>)

c. Keep default settings and click next until user is created. 

d. Generate access key in the AWS Console. Keep keys and paste the access key back into the command line 
![Alt text](<Screenshot 2023-11-25 at 6.35.05 AM.png>)![Alt text](<Screenshot 2023-11-25 at 6.36.38 AM.png>)![Alt text](<Screenshot 2023-11-25 at 6.37.14 AM.png>)

e. Add keys in terminal and leave profile name as default
![Alt text](<Screenshot 2023-11-25 at 6.39.59 AM.png>)

f. Amplify CLI is now configured. 

reference: https://www.youtube.com/watch?v=fWbM5DLh25U

5. Initialize the amplify app

a. To deploy the backend and initialize the backend environment locally, use the Amplify console where the app was created earlier and select backend environments tab > get started > launch studio


![Alt text](<Screenshot 2023-11-25 at 6.54.14 AM.png>)

![Alt text](<Screenshot 2023-11-25 at 6.57.11 AM.png>)


b. Expand local setup instructions


![Alt text](<Screenshot 2023-11-25 at 6.58.16 AM.png>)

c. copy and paste into terminal ensuring that you are in the project root path. 

Follow any login instructions, in my case I had to manually copy and paste a login token from the aws console website into the terminal.

d. Configure backend environment with terminal questions.  Configure as follows

```

? Choose your default editor: Visual Studio Code
✔ Choose the type of app that you're building · javascript
Please tell us about your project
? What javascript framework are you using react
? Source Directory Path:  src
? Distribution Directory Path: build
? Build Command:  npm run-script build
? Start Command: npm run-script start
No AppSync API configured. Please add an API
? Do you plan on modifying this backend? Yes

```

![Alt text](<Screenshot 2023-11-25 at 3.42.59 PM.png>)
e. Follow login instructions to login to amplify studio
![Alt text](<Screenshot 2023-11-25 at 7.01.05 AM.png>)

<!-- headings -->
<a id="Chapter_3_add_auth"></a>
# Chapter 3 - Adding Authentication

Will use Amplifies CLI and libraries to add authentication to app.

### Concepts
>Amplify Libraries
Libraries that allows web or mobile apps to interact with AWS services
###
> Authentication 
Used to identify user that created a request
###

    react
### 

1. Install Amplify libraries

a. In order for AWS services to react to client-side APIs, the aws-amplify library will be used.

 Run
```shell
npm install aws-amplify
```

b. Framework specific UI components will be in the @aws-amplify/ui-react library
 Run
```shell
npm install @aws-amplify/ui-react
```


2. Creating an Authentication Service

a. Configure auth service.
```shell
amplify add auth

 Do you want to use the default authentication and security configuration? Default configuration
 How do you want users to be able to sign in? Username
 Do you want to configure advanced settings? No, I am done.
```

b. Deploy auth service.
```shell
amplify push --y
```

c. Configure react project with the amplify resources 

To configure the react app , open the src/index.js script and add to the import section.
```javascript
import { Amplify } from 'aws-amplify';
import config from './aws-exports';
Amplify.configure(config);
```

![Alt text](<Screenshot 2023-11-25 at 4.00.37 PM.png>)

d. Add the authentication workflowe to the react app

To add authentication workflow, open src/App.js and add the following code:


Default code was this: 
```javascript
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;

```
Which published this 
![Alt text](<Screenshot 2023-11-25 at 4.06.03 PM.png>)

Delete default src/App.js script contents and update with the following

```javascript
import logo from "./logo.svg";
import "@aws-amplify/ui-react/styles.css";
import {
  withAuthenticator,
  Button,
  Heading,
  Image,
  View,
  Card,
} from "@aws-amplify/ui-react";

function App({ signOut }) {
  return (
    <View className="App">
      <Card>
        <Image src={logo} className="App-logo" alt="logo" />
        <Heading level={1}>We now have Auth!</Heading>
      </Card>
      <Button onClick={signOut}>Sign Out</Button>
    </View>
  );
}

export default withAuthenticator(App);
```
To view the app locally, run:
```shell
npm start
```
![Alt text](<Screenshot 2023-11-25 at 4.13.50 PM.png>)
![Alt text](<Screenshot 2023-11-25 at 4.13.39 PM.png>)

This app now has the authentication workflow by using the withAuthenticator workflow. Allows sign up, sign in, password reset, and multifactor auth. A sign out button option that allows users to sign out.



2. Setting up CI/CD for the full-stack 

a. To view the app locally, run:
```
amplify console
Which site do you want to open? AWS console
```
b. Sign in to the AWS Console
![Alt text](<Screenshot 2023-11-25 at 4.32.35 PM.png>)

c. Click App settings > Build settings
![Alt text](<Screenshot 2023-11-25 at 4.39.44 PM.png>)

d. Update app build specification file, amplify.yaml, with following. This change will configure the build specifications to add the backend to the Continuos Deployment(CD) workflow. 

```shell
version: 1
backend:
  phases:
    build:
      commands:
        - '# Execute Amplify CLI with the helper script'
        - amplifyPush --simple
frontend:
  phases:
    preBuild:
      commands:
        - yarn install
    build:
      commands:
        - yarn run build
  artifacts:
    baseDirectory: build
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```
![Alt text](<Screenshot 2023-11-25 at 4.43.31 PM.png>)



 Go to Build image settings > Edit

![Alt text](<Screenshot 2023-11-25 at 4.50.11 PM.png>)

Then, scroll down and click Add package version everride > Amplify CLI and save.
![Alt text](<Screenshot 2023-11-25 at 4.56.27 PM.png>)

e. Connect the front end branch to the backend environment that was just created.

Under the app hosting environment section, edit the continuous deployment set up under the master branch.
![Alt text](<Screenshot 2023-11-25 at 4.59.44 PM.png>)

Select the staging backend environment we just created to connect front end to backend. The front-end wil now be connected to the backend. 
![Alt text](<Screenshot 2023-11-25 at 5.01.41 PM.png>)

Follow instructions to setup the service role in Amplify, https://docs.aws.amazon.com/amplify/latest/userguide/how-to-service-role-amplify-console.html.

Choose the Amplify service and the use case as Amplify - Backend Deployment.
![Alt text](<Screenshot 2023-11-25 at 5.05.31 PM.png>)
Accept default permissions
![Alt text](<Screenshot 2023-11-25 at 5.09.55 PM.png>)
Add a role name and description. In this case the ole name is AmplifyConsoleServiceRole-AmplifyRole. It will allow Amplify Backend Deployment to access AWS resources on your behalf. 
![Alt text](<Screenshot 2023-11-25 at 5.13.20 PM.png>)

Scroll down and create role.

f. Add permission to Amplify Console to deploy backend resources. 

Go back to AWS Amplify Console under the app we are creating.

Click App settings > General > Edit
![Alt text](<Screenshot 2023-11-25 at 5.15.00 PM.png>)

Add the role AmplifyConsoleServiceRole-AmplifyRole to the app. 
![Alt text](<Screenshot 2023-11-25 at 5.19.32 PM.png>)

g. Add Confused Deputy Prevention 

For security completeness, if there all multiple service roles it is good practice to update each roles trust policy to protect against confused deputy condition. Since we dont have multiple service roles, I will not add this but the documentation to update the role is here, https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html.

3. Pushing changes to Github to deploy to live environment. 

I am using VSCode but you could manually push changes with 
```shell
git add .
git commit -m "add authentication to App.js"
git push origin main
```

### Summary of Chapter 3


1. Added authentication by updating App.js script in react project.
2. Update app build specification file, amplify.yaml,via the Amplify Console to configure the build specifications to add the backend to the Continuos Deployment(CD) workflow.
3. Connected front-end to back-end via the Amplify Console.


<!-- headings -->
<a id="Chapter_4_add_api_db"></a>
# Chapter 4 - Adding GraphQL API and Database

### Overview

Will add an API to the app via the Amplify CLI and libraries. The app that is going to be created is a notes app that allows users to createm, delete and list notes. 

references: https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/module-four/

note: I am following this Amazon learning resource to learn the proper way of setting up a full-stack environment with AWS. 


### 

                Application Programming Interface (API) Concept
    APIs are mechanisms that allow two software programs or applications(Server and Client) to communicate with each other. They have custom contracts that setup the structure of requests and responses. 
    source: https://aws.amazon.com/what-is/api/. 


    An API analogy I like is ordering food at a restaurant. There is the client(customer), chef(server), and the waiter(API). The customer(client) requests an order from the chef(server) via the waiter(API). 

    - How will it be used in this project?

### 

              GraphQL Concept
    GraphQL is a query and manipulation language for APIs. It is a language that structures and describes data requirements and interactions. GraphQL defines the shape of the output within the query itself. It was developed by Facebook and open-sourced in 2015. 
    sources: https://graphql.org/learn/ and https://aws.amazon.com/what-is/api/

Example of a typical GraphQL query:
```GraphQL
query {
 post(title: "How does GraphQL work?") {
   id
   author {
     name
     profile_url
   }
   content
  }
}
```
Example of a typical GraphQL query result:
```GraphQL
{
  "post": {
    "id": 123,
    "author": {
      "name": "Jeff Barr",
      "profile_url": "https://aws.amazon.com/blogs/aws/author/jbarr/"
     },
    "content": "GraphQL gives the API users the flexibility..."
  }
}
```
### 

                AWS AppSync Concept
    AppSync is a fully managed GraphQL API service. It connects apps to multiple data sources. Simplifies data access by a unified API that provides a single endpoint to securely query/update data from multiple databases, microservices, and APIs. Serverless and scales automatically. Pub/Sub API's can push data updates to subscribed clients over serverless web sockets to clients. 

    source: https://aws.amazon.com/appsync

### 


                Serverless Database Concept 
    Is collection of organized information that decouples storage management from query operations. The advantage is that these two parts can scale independently. The storage can be scaled as needed while the query requests can be scaled at its own pace depending on demand. 

### 

                Amazon DynamoDB Concept

    Is a fully managed, key-value NOSQL, serverless database. DynamoDB has built-in security, continuos development, continuous backups, automateed multi-Region replication, in-memory caching, and data import and export tools. 
    
    source: https://aws.amazon.com/dynamodb/
  
    Difference between NoSQL vs MySQL?

    NoSQL is a non-relational database meaning it does not follow a tabular schema of rows and columns. Instead non-relational databases stores data in an optimized manner depending on the type of data being stored. The term NoSQL (Not only SQL) refers to data stores that do not necessarily use SQL for queries. Some popular models are: 
    - Key-value database where data is stored under keys and values.
    - Document Databases where data is stored in documents similar to JSON objects.
    - Wide-column stores where data is stored and queried using row key, column names, and cell timestamps
    -Graph Databases where data is stored under vertices and edges. Used to visualize and analyze connections between different pieces of data. 

    source: https://medium.com/nerd-for-tech/mysql-vs-nosql-b0838f6ae10


    MySQL is a relational database that follows a tabular schema of rows and columns to store data. MySQL is more popular since it's been around longer and is open source. 

    source: https://learn.microsoft.com/en-us/azure/architecture/data-guide/big-data/non-relational-data

### Steps

1. Create GraphQL API and database 

a. Run the following code from the root of the app directory and answer questions. 

```shell
amplify add api

? Select from one of the below mentioned services: GraphQL
? Here is the GraphQL API that we will create. Select a setting to edit or continue: Continue
? Choose a schema template: Single object with fields: (e.g., “Todo” with ID, name, description)
✔ Do you want to edit the schema now? (Y/n):  yes
```

b. Open GraphQL file in app directory. I am using VSCode.
   /amplify/backend/api/<api_name>/schema.graphql.

   ![Alt text](<Screenshot 2023-11-25 at 7.19.44 PM.png>)

c. update the file with following code and save:

```shell
type Note @model @auth(rules: [ { allow: public } ] ){
  id: ID!
  name: String!
  description: String
}
```

![Alt text](<Screenshot 2023-11-25 at 7.20.56 PM.png>)



The GraphQL API has been configured locally. 

2. Deploy local GraphQL API.

a. run the following code to build local backend and provision in the cloud.

"amplify push" will build all the local backend resources and provision it in the cloud.

This will create three things 
 - Create the AWS AppSync API
 - Create a DynamoDB table
 - Create a local GraphQL script located at src/graphql

```shell
amplify push --y
```

After the files are built. The back-end will successfully deploy.



To view GraphQL API run the following code and select GraphQL API

```shell
amplify console api 

> Choose GraphQL
```
This opens the following UI console

![Alt text](<Screenshot 2023-11-25 at 7.32.12 PM.png>)

To view Amplify app run the following code and select AWS Console

```shell
amplify console

> AWS Console
```

This opens the amplify app in AWS Console

![Alt text](<Screenshot 2023-11-25 at 7.34.27 PM.png>)


3. Write frontend code to interact with the API

a. Now that the backend has been deployed, we update src/App.js script to add functionality to the front-end of the app. The following code for App.js has been sourced from the AWS resource center. I will create my own react project in the future. This is for tutorial sake. 
source: https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/module-four/

Update src/App.js with: 
```javascript
import React, { useState, useEffect } from "react";
import "./App.css";
import "@aws-amplify/ui-react/styles.css";
import { Amplify } from "aws-amplify";
import {
  Button,
  Flex,
  Heading,
  Text,
  TextField,
  View,
  withAuthenticator,
} from "@aws-amplify/ui-react";
import { listNotes } from "./graphql/queries";
import {
  createNote as createNoteMutation,
  deleteNote as deleteNoteMutation,
} from "./graphql/mutations";

const App = ({ signOut }) => {
  const [notes, setNotes] = useState([]);

  useEffect(() => {
    fetchNotes();
  }, []);

  async function fetchNotes() {
    const apiData = await Amplify.graphql({ query: listNotes });
    const notesFromAPI = apiData.data.listNotes.items;
    setNotes(notesFromAPI);
  }

  async function createNote(event) {
    event.preventDefault();
    const form = new FormData(event.target);
    const data = {
      name: form.get("name"),
      description: form.get("description"),
    };
    await Amplify.graphql({
      query: createNoteMutation,
      variables: { input: data },
    });
    fetchNotes();
    event.target.reset();
  }

  async function deleteNote({ id }) {
    const newNotes = notes.filter((note) => note.id !== id);
    setNotes(newNotes);
    await Amplify.graphql({
      query: deleteNoteMutation,
      variables: { input: { id } },
    });
  }

  return (
    <View className="App">
      <Heading level={1}>My Notes App</Heading>
      <View as="form" margin="3rem 0" onSubmit={createNote}>
        <Flex direction="row" justifyContent="center">
          <TextField
            name="name"
            placeholder="Note Name"
            label="Note Name"
            labelHidden
            variation="quiet"
            required
          />
          <TextField
            name="description"
            placeholder="Note Description"
            label="Note Description"
            labelHidden
            variation="quiet"
            required
          />
          <Button type="submit" variation="primary">
            Create Note
          </Button>
        </Flex>
      </View>
      <Heading level={2}>Current Notes</Heading>
      <View margin="3rem 0">
        {notes.map((note) => (
          <Flex
            key={note.id || note.name}
            direction="row"
            justifyContent="center"
            alignItems="center"
          >
            <Text as="strong" fontWeight={700}>
              {note.name}
            </Text>
            <Text as="span">{note.description}</Text>
            <Button variation="link" onClick={() => deleteNote(note)}>
              Delete note
            </Button>
          </Flex>
        ))}
      </View>
      <Button onClick={signOut}>Sign Out</Button>
    </View>
  );
};

export default withAuthenticator(App);
```
Note: the code provided from AWS resource center is depreciated. I have fixed the problem. 
import { API } from "aws-amplify"; changes to import { Amplify } from "aws-amplify";

The code provided by the AWS learning resource center has three main functions: 

- fetchNotes: This function uses the API class to send a query to the GraphQL API and retrieve a list of notes. 

- createNote: This function uses the API class to send an mutation to the GraphQL API. This function passes variables to GraphQL to create a note with form data.

- deleteNote: This function uses the API class to pass variables as a GraphQL mutation to delete a note. 



b. run the code with:

```shell
npm start
```

### Summary of Chapter 4


1. Overview of AWS AppSync, Amazon DynamoDB, NoSQL, APIs, and GraphQL concepts.
2. Created GraphQL API and database 
3. Deployed GraphQL API and database 
4. Updated App.js with AWS learning resources frontend code note app to interact with the API 


<!-- headings -->
<a id="Chapter_5"></a>
# Chapter 5 - Adding Storage and Associated Image


### Overview

Will add storage functionality to app by using Amplify CLI libraries and Amazon S3.
Will update GrapghQL schema format to associate each image with note.
Will update the react app to enable image uploading, fetching and rendering. 

references: https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/module-five/?e=gs2020&p=build-a-react-app-four

note: I am following this Amazon learning resource to learn the proper way of setting up a full-stack environment with AWS. 


### 

                Amazon Simple Storage Service (Amazon S3)
    Our app requires a storage mechanism/service for uploading images, fetching, and rendering. One option is to encode the files and send as a string to save in database. This is disadvantages because of the encoded file size, the computational expense, and complexity of encoding/decoding. Instead a specialized storage service such as, Amazon S3 can be used.

### Steps

1. Create Storage Service with Amplify

Amplify storage category will be used to add storage funcionality to our app. Use the Amplify CLI to choose the configuration in the following way: 

```shell 
amplify add storage
? Select from one of the below mentioned services: Content (Images, audio, video, etc.)
✔ Provide a friendly name for your resource that will be used to label this category in the project: · imagestorage
✔ Provide bucket name: · <unique_bucket_name>
✔ Who should have access: · Auth users only
✔ What kind of access do you want for Authenticated users? · create/update, read, delete
✔ Do you want to add a Lambda Trigger for your S3 Bucket? (y/N) · no
```

![Alt text](<Screenshot 2023-11-25 at 8.30.13 PM.png>)


2. Updating GraphQL Schema in app codebase 

Update the amplify/backend/api/notesapp/schema.graphql script with the following schema and save:

```graphql
type Note @model @auth(rules: [ { allow: public } ] ){
  id: ID!
  name: String!
  description: String
  image: String
}
```

![Alt text](<Screenshot 2023-11-25 at 8.33.47 PM.png>)

Changes for storage service and updating GraphQL schema have now been successfully implemented.

3. Deploy Local Storage Service Additions and Local GraphQL updates. 

Run the following command: 

```bash 
amplify push --y
```

Once confirmed in terminal, the changes will have successfully been deployed. 

4. Update the React App 

The backend storage and GraphQL services have been deployed, the React app front end will be updated to provide functionality to upload and view images from a note. 

Open src/App.js and make the following changes:


a. Add the storage class and image component to the Amplify imports: 


```javascript
import { Amplify, Storage } from 'aws-amplify';
import {
  Button,
  Flex,
  Heading,
  Image,
  Text,
  TextField,
  View,
  withAuthenticator,
} from '@aws-amplify/ui-react';
```


b. Update the fetchNotes function to fetch an image if there is an image associated with note. 


```javascript
async function fetchNotes() {
  const apiData = await Amplify.graphql({ query: listNotes });
  const notesFromAPI = apiData.data.listNotes.items;
  await Promise.all(
    notesFromAPI.map(async (note) => {
      if (note.image) {
        const url = await Storage.get(note.name);
        note.image = url;
      }
      return note;
    })
  );
  setNotes(notesFromAPI);
}
```

c. Update createNote function to add the image to the local image array if an image is associated with the note. 

```javascript
async function createNote(event) {
  event.preventDefault();
  const form = new FormData(event.target);
  const image = form.get("image");
  const data = {
    name: form.get("name"),
    description: form.get("description"),
    image: image.name,
  };
  if (!!data.image) await Storage.put(data.name, image);
  await Amplify.graphql({
    query: createNoteMutation,
    variables: { input: data },
  });
  fetchNotes();
  event.target.reset();
}
```

d. Update deleteNote function to delete files from storage when notes are deleted:

```javascript
async function deleteNote({ id, name }) {
  const newNotes = notes.filter((note) => note.id !== id);
  setNotes(newNotes);
  await Storage.remove(name);
  await Amplify.graphql({
    query: deleteNoteMutation,
    variables: { input: { id } },
  });
}
```

e. Add additional input "image: to the from in the return block: 


```javascript

```

























