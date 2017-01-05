#Sheets API v4

Allows you to start working with the Google Sheets API v4 right away.


A Google Sheets API v4 implementation that abstracts the authorization portion of each request away from the user a bit. Simply call authorize to get a promise that resolves the OAuth2 client which can be used to make requests. Provides generic 'collection' methods that assist with calling methods associated with those collections i.e. 'values' maps to spreadsheets.values. Really just a wrapper around googleapis.

Here's how to use it:

Visit [Node quickstart](https://developers.google.com/sheets/api/quickstart/nodejs) and complete Step 1 to get your client_secret.json.

```bash
npm i sheets-api --save
```
```javascript
// sheets-client.js
const SheetsAPI = require('sheets-api');
const sheets = new SheetsAPI();

// ValueRange(https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values#ValueRange)
let payload = {
  spreadsheetId: "19uTpwB-PM9TtUGUCRMdgWdkH0pqEHVv3J6sjzCNoMRM",
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
  // Oh I'm not done. Need to append again. Should I do anything using the
  // response from the first request? Naw.. I'm good.
  .then((auth, resp) => sheets.values('append', auth, payload))
  // Derp
  .catch(e => console.error(e))
```

![Zoolander - Its so simple](https://cdn.meme.am/cache/instances/folder28/500x/65581028.jpg)
