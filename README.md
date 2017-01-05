#Sheets API v4

Allows you to start working with the Google Sheets API v4 right away.


A Google Sheets API v4 implementation that abstracts the authorization portion of each request away from the user a bit. Simply call authorize to get a promise that resolves the OAuth2 client which can be used to make requests. Provides generic 'collection' methods that assist with calling methods associated with those collections i.e. 'values' maps to spreadsheets.values. Really just a wrapper around googleapis.

