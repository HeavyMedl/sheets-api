# Sheets API v4

Allows you to start working with the Google Sheets API v4 right away.


A Google Sheets API v4 implementation that abstracts the authorization portion of each request away from the user a bit. Simply call authorize to get a promise that resolves the OAuth2 client which can be used to make requests. Provides generic 'collection' methods that assist with calling methods associated with those collections i.e. 'values' maps to spreadsheets.values. Really just a wrapper around googleapis.

Here's how to use it:

Visit [Node quickstart](https://developers.google.com/sheets/api/quickstart/nodejs) and complete Step 1 to get your `credentials.json`.

```bash
npm i sheets-api --save
```
```javascript
// sheets-client.js
const SheetsAPI = require('sheets-api');
const sheets = new SheetsAPI();
const SPREADSHEET_ID = "19uTpwB-PM9TtUGUCRMdgWdkH0pqEHVv3J6sjzCNoMRM";

// ValueRange(https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values#ValueRange)
let payload = {
  spreadsheetId: SPREADSHEET_ID,
  range: "Orders!A1:D1",
  valueInputOption: 'USER_ENTERED',
  resource : {
    majorDimension: "ROWS",
    values: [
      ["Door", "$15", "2", "3/15/2017"],
      ["Engine", "$100", "1", "3/20/2016"]
    ]
  }
}

sheets
  // Get me an authorized OAuth2 client thats ready to make requests.
  .authorize()

  // https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values/append
  // Using the spreadsheets.values collection, use the 'append' method to
  // append data (payload) to a spreadsheet.
  .then(auth => sheets.values('append', auth, payload))

  // 'response' is an object that contains the response from the request to the
  // API (append) and the OAuth2 client to be chained further. It looks like this:
  // {auth:auth, response:response}
  .then(response => sheets.values('get', response.auth, {
    spreadsheetId: SPREADSHEET_ID,
    range: 'Orders!A1:D1'
  }))

  // The results of the request to spreadsheets.values.get collection.
  .then(response => {
    console.log(response.response);
  })

  // Derp
  .catch(e => console.error(e))
```

## Change Log

### 1.0.8 - Feb 7, 2019

Added the credentialsPath to path to GoogleAuthorize so you can choose where youd like to store credentials.json. For example:

```javascript
const sheets = new SheetsAPI('path/to/credentials.json');
```

### 1.0.5 - Jan 8, 2017
Fixing typo where response was undefined.

### 1.0.4 - Jan 8, 2017
Publishing README.md to NPM.

### 1.0.3 - Jan 8, 2017
Fixed fn#collections not correctly resolving the response from sheets api. Promise.resolve takes a single object. You can't simply pass n arguments to it. It has single arity.

### 1.0.2 - Jan 5, 2017
Fixed not returning promises from collection methods.

### 1.0.1 - Jan 5, 2017
Added other collection helper methods (spreadsheets.sheets, spreadsheets).

### 1.0.0 - Jan 4, 2017
Initial publish.
