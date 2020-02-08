# BASIC USER GUIDE

This app is a working example of Autodesk Forge 3-Legged Authentication with Explicit authorisation code grant
See - https://forge.autodesk.com/en/docs/oauth/v2/tutorials/get-3-legged-token/

## App Workflow

- `npm Start` renders index.html which requires the user to Sign In to Autodesk
- User logs in and is then asked to grant permissions to the App (i.e. XiPOW Forge) based on the scopes listed in the config.js file
- Note: oauth parameters are passed using the router from oauth.js (ID, secret, callback URL, scopes)
- Once allowed Autodesk then hands back to the callback URL (listed in config.js)
- The returned URL string includes the authorisation code. This code is then exachanged for a Grant Access Token (GAT)
- You will then be able to use the GAT to make calls to other API endpoints on behalf of the end user (for the scopes they have provided permission)

## Testing outside of App

You can test the API using the following twp steps:

(assumes you have already set up a Autodesk App and have received your Client ID & Secret)

1) Recreate the follow URL replacing the Client ID and Redirect URI that was used when creating your App
https://developer.api.autodesk.com/authentication/v1/authorize?response_type=code&client_id=<<ENTER HERE>>&redirect_uri=https%3A%2F%2F<<ENTER REDIRECT URI>>&scope=data:read

2) Log-in & grant access when requested and you will receive the Code in the response URL string. Then run the following cURL command, again replacing the necessary parameters.

curl -v 'https://developer.api.autodesk.com/authentication/v1/gettoken' -X 'POST' -H 'Content-Type: application/x-www-form-urlencoded' -d 'client_id=<<ENTER HERE>>&client_secret=<<ENTER HERE>>&grant_type=authorization_code&code=<<ENTER CODE RETURNED FROM THE FORGE AUTHORISATION SERVER>>&redirect_uri=<<ENTER REDIRECT URI>>’

This will return the GAT as expected.
