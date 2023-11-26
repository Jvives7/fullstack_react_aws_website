# Chapter 1 - Setup

### Concepts
>amplify
###
> React
###

    react
### 


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

