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
6. The Third Party App uses the refresh token when access token expires.

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

The Authorization Code Flow begins by sending the user, 

### Step 2: Handle the Callback and Extract the Code 

### Step 3: Exchange Code for Tokens 

### Step 4: Make an Authenticated API Request

### Step 5: Refresh the Access Token 

## Scopes Reference

## Error Handling 











