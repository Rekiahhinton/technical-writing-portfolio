---
title: Spotify API with OAuth 2.0 Authentication
---

## Overview

OAuth 2.0 is a universal authorization framework. It allows users to grant third-party access to their sensitive data with proper authentication. This is useful for developers to be able to access data from Spotify users.

This guide teaches REST-familiar developers how to authenticate with the Spotify Web API using OAuth 2.0.

Specifically, this guide covers the Authorization Code Flow of two endpoints: 

1. ``https://accounts.spotify.com/authorize`` - Redirect the User to Spotify's Login for Authorization

2. ``https://accounts.spotify.com/api/token`` - Exchange Authorization Code for Tokens

By the end of this guide, developers will be able to build apps with access a Spotify user's listening history, playlists, saved tracks, and more. 

## How OAuth 2.0 Works 

### Roles 

| Role (Spotify Context) | Description |
| ---- | ---- |
| Resource Owner (Spotify User) | Represents the Spotify user who owns the data and grants permission for the Authorization Server to give the Client access. |
| Client (Third Party App) | Represents the application that wants access to the Resource Owner's personal data. |
| Authorization Server (Spotify's Authorization Server) | Represents the party that presents the login and consent screen to the user, verifies credentials, and issues the authorization code and access tokens to the Client. |
| Resource Server (Spotify's Web API) | Represents the server that delivers the Spotify user's personal data to the Client. |

### OAuth 2.0 and Spotify

Spotify integrates OAuth 2.0 by acting as the Authorization Server and Resource Server. When the Client (Third Party App) wants access to a Spotify user's personal information, the Client directs the user to the Authorization Server that requests the user's permission to share their data with the Client. The Authorization Server shows the Spotify user a login and consent screen, and waits for confirmation.

Once the user confirms, the Authorization Server sends an authorization code to the Client who uses it and a Client Secret key to gain an access token. That token then allows the Client to view the personal data through the Resource Server (Spotify's Web API). 

#### Authorization Code Flow

The Authorization Code follows these steps: 

1. The Third Party App directs the Spotify User to the Authorization Server for access.
2. The Spotify User gives permission to the Authorization Server.
3. Spotify's Authorization Server sends authorization code to the Third Party App.
4. The Third Party App exchanges the authorization code and Client Secret for the access tokens and refresh tokens.
5. The Third Party App uses the access token to request data from Spotify's  Web API.
6. The Third Party App uses the refresh token to generate new access tokens when the original access token expires.

### Key Concepts 

The following terms appear throughout this guide: 

| Term | Description |
| ---- | ---- |
| Access Token | The authorization credential used in API requests to give the Client access to the user's data from Spotify's Web API. |
| Authorization Code | The temporary, single-use code the Authorization Server gives to the Client to exchange for tokens. |
| Client ID | The public identification key that the Authorization Server uses to identify the Client. |
| Client Secret | The private key that the Authorization Server uses to verify the identity of the Client. This key should never be shared.|
| Refresh Token | The token the Client uses to obtain another access token after the original access token has expired. | 
| Scope | Indicates the permissions the Client may access. The Client does not have access to anything besides the scope. |

## Prerequisites 

These prerequisites are necessary to run the Authorization Code Flow: 

1. Spotify Developer Account - [Spotify for Developers](https://developer.spotify.com/)
2. A Registered Spotify App - [Create an App](https://developer.spotify.com/documentation/web-api/concepts/apps) (Includes your Client ID, Client Secret, and redirect URL configuration)
3. Postman - [Sign Up/Login](https://www.postman.com/) or curl - [Download](https://curl.se/download.html)
4. Familiarity with REST and HTTP Requests, but no OAuth 2.0 knowledge required.

## The Authorization Code Flow 

### Step 1: Request User Authorization

The Authorization Code Flow begins by requesting authorization from the resource owner (Spotify user). Once the resource owner grants authorization, they will have logged into Spotify's interface and the authorization code goes to your redirect URL.

#### Parameters Table 

| Parameter | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``client_id`` | string | Required | The public identification key that the Authorization Server uses to identify the Client. |
| ``redirect_uri`` | string | Required | A secure endpoint where the authorization server sends the authorization code or tokens after authentication. |
| ``response_type`` | string | Required | Determines the type of response to send when using the ``/authorize`` endpoint. Fixed value of ``response_type=code`` for Authorization Code Flow. |
| ``scope`` | string | Required | Defines the specific permissions of user data the Third Party has access to when authorized. Prevents excessive user data from being shared. |
| ``show_dialog`` | boolean | Optional | A setting that controls whether or not the Authorization Server shows the authorization screen every time the user logs in. Options are ``show_dialog=true`` and ``show_dialog=false``, which is the default. |
| ``state`` | string | Optional | A random string the Third Party App generates to verify the legitimacy of the authorization request. Prevents cross-site request forgery attacks. | 

#### Constructing the Authorization URL

The Authorization URL includes the Spotify authorization endpoint along with the five parameters, ``client_id``, ``response_type``, ``redirect_uri``, ``scope``, and ``state``:

```
https://accounts.spotify.com/authorize?client_id=YOUR_CLIENT_ID&response_type=code&redirect_uri=YOUR_REDIRECT_URI&scope=user-read-recently-played&state=YOUR_STATE
```

The sixth parameter, ``show_dialog``, is optional and not included in the Authorization URL; however, it may be appended to the URL. The default state is ``show_dialog=false``, but developers may include ``show_dialog=true`` to prompt the consent screen on every authorization request.

#### Browser Workflow

A successful implementation of this step will send the authorization code and redirect the developer to the redirect URL in the browser. The redirect URL will have authorization code appended to the end. 

1. Paste the Authorization URL into the browser.
2. Hit Enter.
3. When the Spotify login and consent screen appears, enter your Spotify credentials and approve the permissions. The browser redirects to the ``redirect_uri``.
4. Copy the value of the ``code`` parameter at the end of the ``redirect_uri`` for use in the next step.

The redirect URL and authorization code should resemble this example:

```
https://oauth.pstmn.io/v1/callback?code=AQD...&state=abc123
```

#### Postman Workflow

Complete these steps in Postman to receive the authorization code:

1. Create a new request in Postman.
2. Click the **Authorization** tab, open **Auth Type** dropdown, and select **OAuth 2.0**.
3. Scroll down to **Configure New Token** and fill in the corresponding boxes to create a new access token. Choose a **Token Name**, then fill in **Auth URL**, **Access Token URL**, **Client ID**, **Client Secret**, and **Scope**. These values should match the values in the Spotify app registration. 
4. Click **Get New Access Token** to register the token.
5. Under the **Token** dropdown, find **Manage Tokens** to view the response fields of your token. The pop-up box confirms the flow was successful and the authorization code exchanged properly. The token is ready for use.

**Note:** Postman automatically attaches the token to your new requests when selecting it from the **Available Tokens** dropdown menu. 

### Step 2: Handle the Callback and Extract the Code 

This step involves extracting the authorization code for future use. 

1. Navigate to the return URL in the browser.

```
https://oauth.pstmn.io/v1/callback?code=AQD...&state=abc123
```

2. Find the authorization code - the value of the ``code`` parameter - at the end of the return URL.

3. Copy the value for use in the next step.

**Note:** The authorization code is single-use and temporary. If the code is not applied, the Authorization Code Flow must start over from the beginning. This means any error in the exchange request or delay in exchanging the code for tokens would require resetting the process.

### Step 3: Exchange Code for Tokens 

This step involves exchanging the authorization code in the previous step for access tokens from Spotify's authorization server. 

#### Parameters Table 

| Parameter | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``client_id`` | string | Required | The public identification key that the Authorization Server uses to identify the Client.|
| ``client_secret`` | string | Required | The private key that the Authorization Server uses to verify the identity of the Client. This key should never be shared. |
| ``code`` | string | Required | The authorization code recorded from the ``redirect_uri`` and submitted to receive the access and refresh tokens. |
| ``grant_type`` | string | Required | Determines which OAuth 2.0 flow is being used for the token exchange. For the Authorization Code Flow, the value is always ``authorization_code``. |
| ``redirect_uri`` | string | Required | Confirms the application making the token exchange is the same application that requested the authorization request. This parameter does not redirect anything; it is merely a security measure. |

#### curl Request

```
curl --location "https://accounts.spotify.com/api/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=authorization_code" -d "code=YOUR_AUTHORIZATION_CODE" -d "redirect_uri=https://oauth.pstmn.io/v1/callback" -d "client_id=YOUR_CLIENT_ID" -d "client_secret=YOUR_CLIENT_SECRET" 
```

#### Postman Workflow

1. Create a new request in Postman.
2. Set the request to POST https://accounts.spotify.com/api/token
3. Confirm the authorization on the **Authorization** tab is set to **OAuth 2.0** and contains your current token.
   **Note:** The token must be active. Tokens are set to expire within one hour of creation by default.
5. Set the body parameters to **x-www-form-urlencoded**.
6. Fill in the body parameters:
   ``grant_type=authorization_code`` 
   ``code=YOUR_AUTHORIZATION_CODE``
   ``redirect_uri=https://oauth.pstmn.io/v1/callback``
   ``client_id=YOUR_CLIENT_ID``
   ``client_secret=YOUR_CLIENT_SECRET``

7. Hit **Send**.


   
### Step 4: Make an Authenticated API Request

#### curl Request

```
curl --location "https://api.spotify.com/v1/me/player/recently-played" --header "Authorization: Bearer your_bearer_token"
```

### Step 5: Refresh the Access Token 

## Scopes Reference

## Error Handling 











