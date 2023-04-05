# Zoom Meeting SDK Auth Endpoint sample

Use of this sample app is subject to our [Terms of Use](https://explore.zoom.us/en/legal/zoom-api-license-and-tou/).

---

**NOTE:** This sample app has been updated to use [Meeting SDK app type](https://developers.zoom.us/docs/meeting-sdk/create/) credentials instead of [JWT app type](https://developers.zoom.us/docs/platform/build/jwt-app/) type credentials.

---

This is a Node.js / Express server that generates a [Meeting SDK JWT](https://developers.zoom.us/docs/meeting-sdk/auth/#generate-a-meeting-sdk-jwt) via an http request for authorized use of the Meeting SDK.

If you would like to skip these steps and just deploy the finished code to Heroku, click the Deploy to Heroku button. (You will still need to configure a few simple things, so skip to [Deployment](#deployment).)

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/zoom/meetingsdk-auth-endpoint-sample)

## Installation

In terminal, run the following command to clone the repo:

`$ git clone https://github.com/zoom/meetingsdk-auth-endpoint-sample.git`

## Setup

1. In terminal, cd into the cloned repo:

   `$ cd meetingsdk-auth-endpoint-sample`

1. Then install the dependencies:

   `$ npm install`

1. Create an environment file to store your Meeting SDK credentials:

   `$ touch .env`

1. Add the following code to the `.env` file, and insert your Zoom Meeting SDK Key and Secret or Client ID and Client Secret for Meeting SDK app type's created after February 11, 2023, found on the App Credentials page in the Zoom App Marketplace:

   ```
   ZOOM_MEETING_SDK_KEY=MEETING_SDK_KEY_HERE
   ZOOM_MEETING_SDK_SECRET=MEETING_SDK_SECRET_HERE
   ```

1. Save and close `.env`.

1. Start the server:

   `$ npm run start`

## Usage

Make a POST request to `http://localhost:4000` (or your deployed url) with the following request body:

| Key                   | Value Description |
| -----------------------|-------------|
| meetingNumber          | Required, the Zoom Meeting or Webinar Number. |
| role                   | Required, `0` to specify participant, `1` to specify host.  |

### Example Request

POST `http://localhost:4000`

Request Body:

```json
{
  "meetingNumber": "123456789",
  "role": 0
}
```

If successful, the response body will be a JSON representation of your Meeting SDK JWT:

```json
{
  "signature": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzZGtLZXkiOiJhYmMxMjMiLCJtbiI6IjEyMzQ1Njc4OSIsInJvbGUiOjAsImlhdCI6MTY0NjkzNzU1MywiZXhwIjoxNjQ2OTQ0NzUzLCJhcHBLZXkiOiJhYmMxMjMiLCJ0b2tlbkV4cCI6MTY0Njk0NDc1M30.UcWxbWY-y22wFarBBc9i3lGQuZAsuUpl8GRR8wUah2M"
}
```

In the [Meeting SDK](https://developers.zoom.us/docs/meeting-sdk/auth/#join-meetings-and-webinars-with-the-meeting-sdk-jwt), pass in the `signature` to the `join()` function:

```js
// Make http request to your auth endpoint to get the Meeting SDK JWT

// Meeting SDK - web - Client View - example:
ZoomMtg.join({
  signature: signature,
  sdkKey: sdkKey,
  userName: userName,
  meetingNumber: meetingNumber,
  passWord: password
})

// Meeting SDK - web - Component View - example:
client.join({
  signature: signature,
  sdkKey: sdkKey,
  userName: userName,
  meetingNumber: meetingNumber,
  password: password
})
```

## Deployment

If you used the Deploy to Heroku button, enter a name for your app on the page the button took you to (or leave it blank to have a name generated for you), and fill in the values for these,

- `ZOOM_MEETING_SDK_KEY` (Your Zoom Meeting SDK Key or Client ID for Meeting SDK app type's created after February 11, 2023, found on your Zoom Meeting SDK App Credentials page)
- `ZOOM_MEETING_SDK_SECRET` (Your Zoom Meeting SDK Secret or Client Secret for Meeting SDK app type's created after February 11, 2023, found on your Zoom Meeting SDK App Credentials page)

Then click "Deploy App".

Now you can generate [your Meeting SDK JWT](#usage) via the deployed url Heroku provides.

If you cloned this repo, use the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) to deploy your server.

1. In terminal, create a Heroku app:

   `$ heroku create`

1. Add your files:

   `$ git add -A`

1. Commit your files:

   `$ git commit -m "deploying to heroku"`

1. Deploy your server by pushing your files to Heroku:

   `$ git push origin heroku`

1. Navigate to your app on the Heroku dashboard, click settings, and add your SDK App Credentials in the Config Variables,

   - `ZOOM_MEETING_SDK_KEY` (Your Zoom Meeting SDK Key or Client ID for Meeting SDK app type's created after February 11, 2023, found on your Zoom Meeting SDK App Credentials page)
   - `ZOOM_MEETING_SDK_SECRET` (Your Zoom Meeting SDK Secret or Client Secret for Meeting SDK app type's created after February 11, 2023, found on your Zoom Meeting SDK App Credentials page)

Now you can generate [your Meeting SDK JWT](#usage) via the deployed url Heroku provides.

## Need help?

If you're looking for help, try [Developer Support](https://devsupport.zoom.us) or our [Developer Forum](https://devforum.zoom.us). Priority support is also available with [Premier Developer Support](https://explore.zoom.us/docs/en-us/developer-support-plans.html) plans.
