---
id: create-oauth-provider
title: Create a OAuth2 Provider
---

<p align="center">
<img height="50dp" src="https://github.com/goauthentik/authentik/raw/3ecc715e91ed0bb8b019d2a8fe42d0eb6531a341/web/icons/icon_left_brand.svg"/><br><br>
<img src="https://img.shields.io/badge/Importance-High-critical?style=for-the-badge"/>
</p>

# Creating a `OAuth2/OpenID` Provider

1. Open the administrative UI of your authentik installation <br>
    `https://<your-authentik-binding>/if/admin/`
    <details>
    <summary>Click to show/hide screenshot</summary>

    ![](../img/authentik-admin-overview.png)
    </details>

2. Now navigate to the Providers using the left sidebar `Applications/Providers`
    <details>
    <summary>Click to show/hide screenshot</summary>

    ![](../img/authentik-provider-overview.png)
    </details>

3. Now Create a new `OAuth2/OpenID` provider with the client type set to 
    `public`.

4. Now set the `Client ID` in the frontend as described in the aproppriate documentation