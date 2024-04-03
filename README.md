# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Build a Quiz App using AWS Amplify and Cognito (with CI/CD)


## <a name="introduction">🤖 Introduction</a>

In this hands-on tutorial, I’ll show you how to build a simple React app (a quiz app) using AWS Amplify and Cognito.  We’ll also see how to set up continuous integration and continuous deployment (CI/CD) using GitHub.


## <a name="design">📐 Design</a>

![React App](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/1f0e8ac3-6c83-4f5f-9a60-a42722167424)

## <a name="steps">☑️ Steps</a>

* Set up environment
* Create the React App
* Add authentication with Cognito
* Add functionality and styling for quiz
* Push local code to GitHub
* Host the front end in Amplify via GitHub (for CI/CD)


## <a name="quick-start">🤸 Quick Start</a>

Follow these steps to set up the project locally on your machine.

**Prerequisites**

Make sure you have the following installed on your machine:

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en)
- [npm](https://www.npmjs.com/) (Node Package Manager)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)


## <a name="snippets">🕸️ Code Snippets</a>

<details>
<summary><code>Quiz.js</code></summary>

```javascript
import React, { useState } from 'react';
import quizData from './quizData';

function Quiz() {
  const [currentQuestion, setCurrentQuestion] = useState(0);
  const [score, setScore] = useState(0);
  const [showScore, setShowScore] = useState(false);
  const [selectedAnswer, setSelectedAnswer] = useState(""); 
  const [isCorrect, setIsCorrect] = useState(null);

  const handleAnswerOptionClick = (option) => {
    const correctAnswer = quizData[currentQuestion].answer;
    setSelectedAnswer(option);
    if (option === correctAnswer) {
      setScore(score + 1);
      setIsCorrect(true);
    } else {
      setIsCorrect(false);
    }

    // Delay moving to the next question to allow the user to see feedback
    setTimeout(() => {
      const nextQuestion = currentQuestion + 1;
      if (nextQuestion < quizData.length) {
        setCurrentQuestion(nextQuestion);
        setIsCorrect(null); // Reset for the next question
        setSelectedAnswer(""); // Reset selected answer
      } else {
        setShowScore(true);
      }
    }, 1000); // Adjust time as needed
  };

  return (
    <div className='quiz'>
      {showScore ? (
        <div className='score-section'>
          You scored {score} out of {quizData.length}
        </div>
      ) : (
        <>
          <div className='question-section'>
            <div className='question-count'>
              <span>Question {currentQuestion + 1}</span>/{quizData.length}
            </div>
            <div className='question-text'>{quizData[currentQuestion].question}</div>
          </div>
          <div className='answer-section'>
            {quizData[currentQuestion].options.map((option) => (
              <button 
                onClick={() => handleAnswerOptionClick(option)} 
                key={option}
                style={{ backgroundColor: selectedAnswer === option ? (isCorrect ? 'lightgreen' : 'pink') : '' }}
              >
                {option}
              </button>
            ))}
          </div>
          {selectedAnswer && (
            <div style={{ marginTop: '10px' }}>
              {isCorrect ? 'Correct! 🎉' : 'Sorry, that’s not right. 😢'}
            </div>
          )}
        </>
      )}
    </div>
  );
}

export default Quiz;
```
</details>

<details>
<summary><code>App.js</code></summary>

```javascript
import React from 'react';
import './App.css';

// Imports the Amplify library from 'aws-amplify' package. This is used to configure your app to interact with AWS services.
import {Amplify} from 'aws-amplify';

// Imports the Authenticator and withAuthenticator components from '@aws-amplify/ui-react'.
// Authenticator is a React component that provides a ready-to-use sign-in and sign-up UI.
// withAuthenticator is a higher-order component that wraps your app component to enforce authentication.
import { Authenticator, withAuthenticator } from '@aws-amplify/ui-react';

// Imports the default styles for the Amplify UI components. This line ensures that the authenticator looks nice out of the box.
import '@aws-amplify/ui-react/styles.css';

// Imports the awsExports configuration file generated by the Amplify CLI. This file contains the AWS service configurations (like Cognito, AppSync, etc.) specific to your project.
import awsExports from './aws-exports';

// Imports the Quiz component from Quiz.js for use in this file.
import Quiz from './Quiz';

// Configures the Amplify library with the settings from aws-exports.js, which includes all the AWS service configurations for this project.
Amplify.configure(awsExports);

function App() {
  return (
    <div className="App">
      <Authenticator>
        {({ signOut }) => (
          <main>
            <header className='App-header'>
              {/* Quiz Component */}
              <Quiz />
              {/* Sign Out Button */}
              <button 
                onClick={signOut} 
                style={{ 
                  margin: '20px', 
                  fontSize: '0.8rem', 
                  padding: '5px 10px', 
                  marginTop: '20px'
                }}
              >
                Sign Out
              </button>
            </header>
          </main>
        )}
      </Authenticator>
    </div>
  );
}

export default withAuthenticator(App);
```
</details>

<details>
<summary><code>quizData.js</code></summary>

```javascript
const quizData = [
    {
      question: "What was the first video game ever made?",
      options: ["Pong", "Spacewar!", "Tetris", "Computer Space"],
      answer: "Spacewar!"
    },
    {
      question: "Which company developed the first commercial antivirus software?",
      options: ["Symantec", "McAfee", "Norton", "Kaspersky Lab"],
      answer: "McAfee"
    },
    {
      question: "Which animal is featured in the official PHP logo?",
      options: ["Elephant", "Hippo", "Giraffe", "Lion"],
      answer: "Elephant"
    },
    {
      question: "What does 'HTTP' stand for?",
      options: ["HyperText Transfer Protocol", "Hyperlink Transfer Technology Protocol", "Hyperlink Text Transfer Protocol", "HyperText Technology Protocol"],
      answer: "HyperText Transfer Protocol"
    },
    {
      question: "Which programming language is known as the backbone of the World Wide Web?",
      options: ["Java", "C#", "Python", "HTML"],
      answer: "HTML"
    },
    {
      question: "What is the name of the world's first computer programmer?",
      options: ["Charles Babbage", "Ada Lovelace", "Alan Turing", "Grace Hopper"],
      answer: "Ada Lovelace"
    },
    {
      question: "In what year was the iPhone first introduced?",
      options: ["2005", "2007", "2009", "2011"],
      answer: "2007"
    },
    {
      question: "What was Google's original name?",
      options: ["BackRub", "Googol", "SearchMaster", "WebSearch"],
      answer: "BackRub"
    },
    {
      question: "Which of these companies was not founded in a garage?",
      options: ["Amazon", "Google", "Apple", "Microsoft"],
      answer: "Amazon"
    },
    {
      question: "What does 'GPU' stand for?",
      options: ["Graphical Processing Unit", "Graphics Performance Unit", "Graphics Processing Unit", "Graphical Performance Unit"],
      answer: "Graphics Processing Unit"
    },  
    {
      question: "What is the capital of France?",
      options: ["New York", "London", "Paris", "Dublin"],
      answer: "Paris"
    },
    {
      question: "Who painted the Mona Lisa?",
      options: ["Vincent Van Gogh", "Leonardo da Vinci", "Pablo Picasso", "Claude Monet"],
      answer: "Leonardo da Vinci"
    },
    {
      question: "What is the largest planet in our solar system?",
      options: ["Earth", "Jupiter", "Saturn", "Mars"],
      answer: "Jupiter"
    }
  ];
  
  export default quizData;
  
```
</details>


## ➡️ Step 1 - Setting up the environment and installing the Amplify CLI

* Open visual studio code and in the terminal we're going to do an npm install for the amplify CLI:
* Copy and paste the command in the terminal `npm install -g @aws-amplify/cli`


![1](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/7a86af16-c2c7-40c9-9432-28281633f324)


* Next we need to configure amplify, this basically makes the CLI work with your AWS accounts, we'll have to set up a new user configure access keys. The command for that its just `amplify configure`
* And that will launch a page to the AWS console asking you to sign in as an administrator account that has admin permissions, after you're signed in you just press enter

![2](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/38ac248c-8e4c-4416-ba89-5a55080d5cd0)


* Choose your region, i'm in `us-east-1`, just use your arrow keys up and down to select your region and then hit enter

![4](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/862de61c-5eca-47fe-8d0c-bf30f04205a0)


* Then this will launch the amplify documentation with instructions on what you need to do next, it's basically giving
you a step- byep of what's happening, we've already run amplify configure and select our region

![5](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/7763462e-8fe7-4c6f-bac5-83e0a76d2d1a)


* Next what we need to do is navigate to the IAM user creation page in the console, and create a user

![6](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/c2abfb5b-952a-45d5-914d-0727e5c49fd1)


* I'm going to call the user `amplify-dev` you can choose another name if you want then click next

![7](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/680bd858-a23d-43fe-be5e-343996e2d4c9)


* Next, for permissions options select `attach policies directly` then select the policy called `AdministratorAccess-Amplify` then click Next
(if you don't see the policy name you can just type in amplify in the search bar and that will filter for you the policy name)

![8](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/43ebf783-f0c7-48d1-ae1b-8cecaf84f788)


* Next you need to enter the access key ID and the secret access key for that user that you just created, back to IAM console select the user you just created


![9](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/11e7a913-4b1d-4010-bbd9-20e67c91eed3)


* Then you want to come to the security credentials Tab and scrolling down to access keys and create one

![11](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/fdcfb227-ed2b-40c7-8b0d-54d2b5d37e67)


* Specifically these will be for the CLI so select that, scroll down understand and accept and click next and create access key. 

![12](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/87984ef4-38fc-467d-99b0-b7260d9a7d21)


* You want to copy the the access key ID and the secret access key go back to visual studio and paste it.

![13](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/7a7af09b-e153-40ba-a0b4-6502b4d3f7af)


* And then you would like to update or create AWS profile on your local machine, i'm going to call mine `amplify-dev-local` and this will save this information like the region, the keys on your local machine so that you don't have to do this every time so we'll hit enter and success

![14](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/6f0fe657-21d6-4c26-a266-ad436cec3332)


## ➡️ Step 2 - Creating a new React app from VS Code

* Back to vs code i'm going to run `npx create-react-app <name of your app>` and then whatever name you want to give it, i'm calling mine `my-quiz-app` this will install a vanilla react app
* And then you want to change directories into whatever you named your app by running `cd <name of your app>`

1. Next we need to initialize the amplify project within the react app, here it's going to ask you a series of questions like what do you want to name your amplify app in the cloud, what language are you using, what's your local IDE, to do that let run `amplify init` (it is recommended to run this from the root we're already in there):

2. The name for the project this is the name that's going to show up in the AWS console when we go look at our amplify projects, it's different from what we just created locally on our file system, i will name it `myquizapp`

3. Then click `Yes` to initailize the project the default configuration (you can select no if you want to customize the configuration)

4. For authentication method, we did save the AWS profile that we set up earlier, you could enter Keys again but let's go with the AWS profile

5. I called my `amplify-dev-local` so I'll select that


![16](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/ebc2313b-9f3b-4591-a8cb-c4080b92d9e0)


6. Next question, do we want to help amplify improve by sharing non-sensitive project configurations i'm going to say No 


![17](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/06337109-a597-45e1-aa01-958fa529abed)


* If we go out to the AWS console, i'll navigate to amplify, we've got my quiz app so this is the one that I just created from the command line

![19](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/8f9fa609-2158-4e50-ba15-1a1227823784)


## ➡️ Step 3 - Adding authentication to the React app using Cognito

1.  To Add authentication to the React app using Cognito, back to vs code in the command line run `amplify add auth`

* This will ask you some questions about what exactly you want to do:

2. We're going to go with a default configuration that will give us Cognito
3. You can do Federation with a social provider like Facebook or Google, you can do manual configuration but we're just going to go
with the default
4. We want users to sign in using an email
5. We don't want to do any advanced settings

![20](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/cefbd17c-472c-4ccc-b1b4-97003a579f69)


* Let's push everything to AWS by running the command `amplify push` then hit continue, and everything will be deployed.

![21](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/def8ff53-d521-4fe8-9d70-4267e41538a2)



* Validating the Cognito user pool was successfully created:

Cognito is built around the concept of a user pool, which is basically a pool of users that are going to register and log into your application, if you check the back to Cognito console, we should see a new user pool created for us

![22](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/46178d2b-6c55-4bc4-b007-4ae61a0a6a57)


* Adding the aws-amplify/ui-react libraries for the Cognito sign-in UI: Let's get the UI set up enough that we can actually create an account and log in as a new user.

* Back to vs code, i'm going to open up the `app.js` file, this is our main file for the react application, i'm going to replace code with my code that i put out in GitHub (grab the code above in "Code Snippets")

* Before test our react app, we do have the quiz functionality in the code in which we haven't implemented yet so let me just comment that out first: its `import Quiz from './Quiz';` and `<Quiz />`

![Screenshot 2024-04-02 at 11 36 31](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/8efc8b4c-a483-44c1-b8a5-160eaba89621)


* There's one other thing we need to do, we need to install the amplify libraries that give us the Login user interface, to do that we will run the command `npm install aws-amplify @aws-amplify/ui-react` then `npm start`

![23](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/63250cf0-817c-4141-a430-9754cec66951)


* Running the React app to register for an account and log in using Cognito, Open http://localhost:3000 in your browser to view the app.

![25](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/3d72bdaf-5162-409a-8688-e08607983f68)

* To create an account or sign in, you do need to use a valid email, it's going to send you a code that you have to use to activate the email


![25](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/4db2061e-8f67-4bcc-8883-1af075345c66)

* I'll enter my email password and create the account, it has emailed me a code i'll go grab that confirmation code, enter that  and confirm, this is a legit user account and that logged me in

![26](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/a39f068c-b353-4e07-869e-54fd1293cb0a)


* Now if we go back to Cognito console and look at our user pool and refresh the users you'll see that we have one user


![27](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/7621ea2d-5517-411d-9586-025cbdab4f8e)


## ➡️ Step 4 - Adding functionality and styling for the quiz

* I do have some code out in our repo (in Code Snippets) that you can grab for quiz, just copy all the code and then back in VS code you will to add a new file under the `src` folder and call it `quiz.js` and then just paste all of that code and save
* Then you want a second file in the same location `src` older, this one is called `quizdata.js` and let me grab that code (in Code Snippets)

N.B: in the real world you'd probably want a database or you want to get these from an API or something like that but we're just keeping things simple for the tutorial.


* Then back in `app.js` we had commented out these two lines, put that back in: `import Quiz from './Quiz';` and `<Quiz /> ` save that file and when you save it your page should refresh, you should see the quiz game 

![28](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/7c88bd86-7e2f-4bf9-8188-53289d3f835d)


## ➡️ Step 5 - Create a new GitHub repository and push local code to it


In the real world, it's more likely that you're going to have your code in GitHub or another source control repository, you're going to hook that up to AWS Amplify and host the frontend online.

We have our code in VS code currently, what we want to do is push that out to a new GitHub repo and then we want to set up amplify to pull from that repo and Amplify will host the frontend. Also whenever we make a change to the code in GitHub we want to trigger a new build and deploy in Amplify, so basically a Continuous Integration Continuous Deployment (CICD Pipeline).

1. Push local code to GitHub, i will create a new repo and name it `aws-amplify-cognito-quiz` 



2. Switch over to visual studio code and run `git init` to initialize the repo for the project
3. Run `git add .` to add everything that we have changed
4. Run `git commit -m "commit message"` to commit all the changes
5. Run `git branch -m main`
6. Run `git remote add origin <repository URL>` paste the repo URL that we just created
7. Finally run `git push –u origin main`

All the code will pushed the new repo from VS code out to GitHub.


## ➡️ Step 6 - Setting up Amplify hosting and CI/CI from GitHub

Now we need to hook setting up Amplify hosting and CI/CD from GitHub up amplify

* Back Cognito console, choose GitHub as hosting environment 

![30](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/f1ec0eac-fe21-4760-af79-cf91be1041b9)


* Then select the Repository mine should be called `aws-amplify-cognito-quiz`

![32](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/4cd8afb1-dfdf-4299-81b9-109e9ab3bbf9)


N.B: If you don't see your repository in the dropdown above, ensure the Amplify GitHub App has permissions to the repository. If your repository still doesn't appear, push a commit and click the refresh button.


![31](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/ebe74580-b43f-4d42-bbd2-624bef2bb636)


* For app name i'll leave this as it (`aws-amplify-cognito-quiz`)
* The backend should be called `myquizapp` 
* Then there should be an environment called `Dev`
* We want to enable full stack CI/CD so anytime we make a change in the GitHub repo that will trigger a new build and deployment   in amplify


![33](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/82a8bff4-220d-44e4-90af-6f74c403e472)


* We need to create a new service role for Amplify Hosting, that gives amplify hosting access to our resources, click on "Create new role"

![33 (1)](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/bbce394d-174d-4610-ae22-d6bba4823f91)

* Then you just basically walk through and take all the defaults
* I will name the role `amplifyconsole-backend-role` scroll down and "Create role"

![36](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/e087eff3-53cb-406f-998c-7f84d7d8b571)
