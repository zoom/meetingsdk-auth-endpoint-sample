# Zoom Web SDK Sample Signature Node.js

This is a Node.js / Express server that generates a [Web SDK signature](https://marketplace.zoom.us/docs/sdk/native-sdks/web/essential/signature) via an http request from your frontend for use in the Web SDK.

If you would like to skip these steps and just deploy the finished code to Heroku, click the Deploy to Heroku button. (You will still need to configure a few simple things, so skip to [Deployment](#deployment).)

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Installation

In terminal, run the following command to clone the repo:

`$ git clone https://github.com/zoom/websdk-sample-signature-node.js.git`

## Setup

1. In terminal, cd into the cloned repo:

   `$ cd websdk-sample-signature-node.js`

1. Then install the dependencies:

   `$ npm install`

1. Create an environment file to store your API Key and Secret:

   `$ touch .env`

1. Add the following code to the `.env` file, and insert your JWT App's API Key and Secret found on the App Crednetials page in the Zoom App Marketplace.:

   ```
   API_KEY=JWT_API_KEY_HERE
   API_SECRET=JWT_API_SECRET_HERE
   ```

1. Save and close `.env`.

1. Start the server:

   `$ npm run start`

## Usage

Make a POST request to `http://localhost:4000` (or your deployed url) with the following request body:

| Body                   | Description |
| -----------------------|-------------|
| meetingNumber          | The Zoom Meeting / Webinar Number. |
| role                   | The role `0` to join the meeting or webinar, or `1` to start the meeting. |

### Example Request

POST `http://localhost:4000`

Request Body:

```json
{
  "meetingNumber": 123456789,
  "role": 0
}
```

If successful, the response body will be a JSON representation of your signature:

```json
{
  "signature": "eHUzSlBhQV9SSlcyLTlsNV9IQWFMQS4xMjM0NTY3ODkuMTU4MzE2OTUzODc3My4wLkJMNEtiM3FINGx5ZzA1MUZtbGJOcGtPRnlFQS9lQUR2bGllVzJNNGZJeWs9"
}
```

In the [Web SDK](https://marketplace.zoom.us/docs/sdk/native-sdks/web/essential/start-join-meeting), pass in the `signature` to the `ZoomMtg.join()` object:

```js
// make http request to your server to get the signature

ZoomMtg.join({
  signature: signature,
  meetingNumber: meetingNumber,
  userName: userName,
  apiKey: apiKey,
  userEmail: userEmail,
  passWord: password,
  success: (success) => {
    console.log(success)
  },
  error: (error) => {
    console.log(error)
  }
})
```

## Deployment

If you used the Deploy to Heroku button, enter a name for your app on the page the button took you to (or leave it blank to have a name generated for you), and fill in the values for these,

- `API_SECRET` (Your Zoom JWT App API Key, found on your Zoom JWT App Credentials page)
- `API_KEY` (Your Zoom JWT App API Secret, found on your Zoom JWT App Credentials page)

Then click "Deploy App".

Now you can generate and [use your signature](#usage) via the deployed url Heroku provides.

If you cloned this repo, use the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) to deploy your server.

1. In terminal, create a Heroku app:

   `$ heroku create`

1. Add your files:

   `$ git add -A`

1. Commit your files:

   `$ git commit -m "deploying to heroku"`

1. Deploy your server by pushing your files to Heroku:

   `$ git push origin heroku`

1. Navigate to your app on the Heroku dashboard, click settings, and add your JWT App Credentials in the Config Variables,

   - `API_SECRET` (Your Zoom JWT App API Key, found on your Zoom JWT App Credentials page)
   - `API_KEY` (Your Zoom JWT App API Secret, found on your Zoom JWT App Credentials page)

Now you can generate and [use your signature](#usage) via the deployed url Heroku provides.

## Need Support?

The first place to look for help is on our [Developer Forum](https://devforum.zoom.us/), where Zoom Marketplace Developers can ask questions for public answers.

If you can’t find the answer in the Developer Forum or your request requires sensitive information to be relayed, please email us at developersupport@zoom.us.
