# Zoom Meeting SDK Auth Endpoint sample

Use of this sample app is subject to our [Terms of Use](https://explore.zoom.us/en/legal/zoom-api-license-and-tou/).

This is a Node.js / Express server that generates a [Meeting SDK JWT](https://developers.zoom.us/docs/meeting-sdk/auth/#generate-a-meeting-sdk-jwt) via an HTTP request for authorized use of the [Zoom Meeting SDK](https://developers.zoom.us/docs/meeting-sdk/).

If you would like to skip these steps and just deploy the finished code to a managed service, click the Deploy to Railway/Render/Heroku button. (You will still need to configure a few simple things, so skip to [Deployment](#deployment).)

| Railway | Render | Heroku |
|:-:|:-:|:-:|
| [![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/JsX6Pk?referralCode=HTPdHX) | [![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/zoom/meetingsdk-auth-endpoint-sample) | [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/zoom/meetingsdk-auth-endpoint-sample) | 

> Note: Both Railway and Render have free tiers, but Heroku requires a credit card to deploy.

## Installation

In terminal, run the following command to clone the repo:

`$ git clone https://github.com/zoom/meetingsdk-auth-endpoint-sample.git`

## Setup

1. In terminal, `cd` into the cloned repository:

   `$ cd meetingsdk-auth-endpoint-sample`

1. Then install the dependencies:

   `$ npm install`

2. Rename `.env.example` to `.env`, edit the file contents to include your [Zoom Meeting SDK key and secret](https://developers.zoom.us/docs/meeting-sdk/developer-accounts/), save the file contents, and close the file.

3. Start the server:

   `$ npm run start`

## Usage

Make a POST request to `http://localhost:4000` (or your deployed url) with the following request body:

| Property            | Type     | Required?  | Validation Rule(s)                                                                          |
| ------------------- | -------- | ---------- | ------------------------------------------------------------------------------------------- |
| `meetingNumber`     | `string` | Yes (web)* | - Required if generating a web JWT, optional for native.                                    |
| `role`              | `number` | Yes (web)* | - Required if generating a web JWT, optional for native. <br> - Must be equal to `0` or `1` |
| `expirationSeconds` | `number` | No         | - Must be between `1800` (30 minutes) and `172800` (48 hours) seconds                       |

> [!IMPORTANT]
> If `meetingNumber` or `role` are supplied in the request body, the other must be present as well. If both are supplied, the JWT will be valid for web, otherwise it will be valid for native.

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

### Deploy to a Managed Service

1. After clicking the "Deploy to <Provider\>" button, enter a name for your app (or leave it blank to have a name generated for you), and insert your [Zoom Meeting SDK credentials](https://developers.zoom.us/docs/meeting-sdk/developer-accounts/#get-meeting-sdk-credentials):

   - `ZOOM_MEETING_SDK_KEY` (Your Zoom Meeting SDK Key or Client ID for Meeting SDK app type's created after February 11, 2023, found on your Zoom Meeting SDK App Credentials page)
   - `ZOOM_MEETING_SDK_SECRET` (Your Zoom Meeting SDK Secret or Client Secret for Meeting SDK app type's created after February 11, 2023, found on your Zoom Meeting SDK App Credentials page)

1. Then click "Deploy App".

1. Use your URL as your Meeting SDK Auth Endpoint.

   Example: `https://abc123.provider.com/`

```bash
$ curl <YOU_URL> -X POST -d '{  "role": "1", "meetingNumber": "123123123"}' -H "Content-Type: application/json"
```
<!-- ### Heroku (CLI)

1. If you cloned this repo, you may use the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) to deploy your server. Remember to [set your config vars (envoirnment variables)](https://devcenter.heroku.com/articles/config-vars).

1. Use your Heroku URL as your Meeting SDK Auth Endpoint.

   Example: `https://abc123.herokuapp.com/` -->

### Other Server Hosting

1. For Other Server Hosting information, see [this tutorial](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/deployment#choosing_a_hosting_provider).

1. Use your deployed URL as your Meeting SDK Auth Endpoint.

   Example: `https://abc123.compute-1.amazonaws.com/`

Now you can [generate your Meeting SDK JWT](#usage).

## Need help?

If you're looking for help, try [Developer Support](https://devsupport.zoom.us) or our [Developer Forum](https://devforum.zoom.us). Priority support is also available with [Premier Developer Support](https://explore.zoom.us/docs/en-us/developer-support-plans.html) plans.
