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
| Authorization Server (Spotify's Authorization Server) | Represents the party who accepts the Authorization Request and Client Secret, and grants tokens to the Client. |
| Resource Server (Spotify's Web API) | Represents the server that delivers the Spotify user's personal data to the Client. |

### OAuth 2.0 and Spotify

Spotify integrates OAuth 2.0 by taking the role of Authorization Server. When the Client (Third Party App) wants access to a Spotify user's personal information, the Client directs the user to the Authorization Server that requests permission from the user to give the third party app access. 

Once the user confirms, the Authorization Server sends an authorization code to the Client who uses it and a Client Secret key to gain an access token. That token then allows the Client to view the personal data through the Resource Server (Spotify's Web API). 

#### Authorization Code Flow

1. Third Party App Directs User to Authorization Server for Access
2. Spotify User Gives Permission to Authorization Server
3. Spotify's Authorization Server Sends Authorization Code to Third Party App
4. Third Party App Exchanges Authorization Code and Client Secret for Access Token and Refresh Token
5. Third Party App Uses Access Token to Request Data from Spotify Web API
6. Third Party App Uses Refresh Token When Access Token Expires

### Key Concepts 

## Prerequisites 

## The Authorization Code Flow 

### Step 1: Request User Authorization

### Step 2: Handle the Callback and Extract the Code 

### Step 3: Exchange Code for Tokens 

### Step 4: Make an Authenticated API Request

### Step 5: Refresh the Access Token 

## Scopes Reference

## Error Handling 











