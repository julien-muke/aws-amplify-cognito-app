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


* Next, for permissions options select `attach policies directly` then select the policy called `AdministratorAccess-Amplify` 
(if you don't see the policy name you can just type in amplify in the search bar and that will filter for you the policy name)

![8](https://github.com/julien-muke/aws-amplify-cognito-app/assets/110755734/43ebf783-f0c7-48d1-ae1b-8cecaf84f788)


* 