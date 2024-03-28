# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Build a Quiz App using AWS Amplify and Cognito (with CI/CD)


## 🤖 Introduction

Learn how to run a Python Docker image on AWS Lambda using Lambda container images.

Using Docker containers for your Python app on AWS Lambda means you can create a special environment for your app with its own tools and settings, making sure it works everywhere, and also making it easy to include all the things (libraries and packages)your app needs to run correctly.


## 📐 Design


![Python Docker Image on AWS Lambda](https://github.com/julien-muke/aws-lambda-docker/assets/110755734/860be5b2-ccd9-4f91-b4ac-b9dfd8d305a5)


## ⚙️ Requirements

* Docker
* AWS CLI
* AWS CDK


## 📋 Steps


👉 Create CDK Project

👉 Create a Python handler app

👉 Create Dockerfile for Lambda

👉 Test the Docker image locally

👉 Create AWS Docker Lambda with CDK  

👉 Deploying to AWS

## 💻 Useful commands

* `npm run build` compile typescript to js
* `npm run watch` watch for changes and compile
* `npm run test` perform the jest unit tests
* `cdk deploy` deploy this stack to your default AWS account/region
* `cdk diff` compare deployed stack with current state
* `cdk synth` emits the synthesized CloudFormation template

## ➡️ Step 1 - Create CDK Project