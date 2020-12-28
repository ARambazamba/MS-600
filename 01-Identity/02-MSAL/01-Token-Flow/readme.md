# Token Flows MSAL.js 1.x and 2.x

Demos require [http-server](https://www.npmjs.com/package/http-server)

- For V1 Demo use `MSAL.js-V1-Token-Flow` app registration

## App Registration

![appreg](_images/app-reg.jpg)

![appreg2](_images/app-reg2.jpg)

## Run Demo

Replace tanant id und client id in `index.js`:

```javascript
async function doAuth() {
  const spTenant = "integrationsonline";
  const config = {
    auth: {
      clientId: "a2d2aa7-c9dc-47ef-899a-2258409bc7c4",
      authority: "https://login.microsoftonline.com/common/",
      redirectUri: "http://localhost:8080",
    },
  };
```

Install http-server:

```
npm i -g http-server
```

> Note: Requires [Note.js](https://nodejs.org/download/release/v10.23.0/)

Run project:

```
cd ./token-flow-node
npm i
http-server
```

> Note: Use http://localhost:8080/ as this is used in the App Registation

Consent Screen:

![consent](_images/consent.jpg)
