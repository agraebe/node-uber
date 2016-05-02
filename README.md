[![build status](https://img.shields.io/travis/shernshiou/node-uber.svg?style=flat-square)](https://travis-ci.org/shernshiou/node-uber) [![npm version](http://img.shields.io/npm/v/gh-badges.svg?style=flat-square)](https://npmjs.org/package/gh-badges) [![Dependency Status](https://david-dm.org/shernshiou/node-uber.svg?style=flat-square)](https://david-dm.org/shernshiou/node-uber) [![devDependency Status](https://david-dm.org/shernshiou/node-uber/dev-status.svg)](https://david-dm.org/shernshiou/node-uber#info=devDependencies)
[![Code Climate](https://codeclimate.com/github/shernshiou/node-uber/badges/gpa.svg)](https://codeclimate.com/github/shernshiou/node-uber) [![Test Coverage](https://codeclimate.com/github/shernshiou/node-uber/badges/coverage.svg)](https://codeclimate.com/github/shernshiou/node-uber/coverage)

node-uber - Node.js wrapper for Uber API
=========

Version History
-------
The change-log can be found in the [Wiki: Version History](https://github.com/shernshiou/node-uber/wiki/Version-History).

License
-------
MIT

Quick Overview
-------
| HTTP Method 	| Endpoint                          	| Auth Method            	| Require Scope                                   	| Method Name                       	| Method Parameters                                                                                         	|
|-------------	|-----------------------------------	|------------------------	|-------------------------------------------------	|-----------------------------------	|-----------------------------------------------------------------------------------------------------------	|
| GET         	| /v1/products                      	| - OAuth - server_token 	|                                                 	| products.getAllForLocation        	| - latitude - longitude - callback function                                                                	|
| GET         	| /v1/products/{product_id}         	| - OAuth - server_token 	|                                                 	| products.getByID                  	| - product_id - callback function                                                                          	|
| GET         	| /v1/estimates/price               	| - OAuth - server_token 	|                                                 	| estimates.getPriceForRoute        	| - start latitude - start longitude - end latitude - end longitude - seats (optional) - callback function  	|
| GET         	| /v1/estimates/time                	| - OAuth - server_token 	|                                                 	| estimates.getETAForLocation       	| - latitude - longitude - product_id (optional) - callback function                                        	|
| GET         	| /v1.2/history                     	| OAuth                  	| - history - history_lite                        	| user.getHistory                   	| - offset (defaults to 0) - limit (defaults to 5, max is 50) - access_token (optional) - callback function 	|
| GET         	| /v1/me                            	| OAuth                  	| profile                                         	| user.getProfile                   	| - access_token (optional) - callback function                                                             	|
| POST        	| /v1/requests                      	| OAuth                  	| request (privileged)                            	| requests.create                   	| - JavaScript object with parameters - callback function                                                   	|
| GET         	| /v1/requests/current              	| OAuth                  	| - request (privileged) - all_trips (privileged) 	| requests.getCurrent               	| - callback function                                                                                       	|
| PATCH       	| /v1/requests/current              	| OAuth                  	| request (privileged)                            	| requests.updateCurrent            	| - JavaScript object with parameters - callback function                                                   	|
| DELETE      	| /v1/requests/current              	| OAuth                  	| request (privileged)                            	| requests.deleteCurrent            	| - callback function                                                                                       	|
| POST        	| /v1/requests/estimate             	| OAuth                  	| request (privileged)                            	| requests.getEstimates             	| - JavaScript object with parameters - callback function                                                   	|
| GET         	| /v1/requests/{request_id}         	| OAuth                  	| request (privileged)                            	| requests.getByID                  	| - request_id - callback function                                                                          	|
| PATCH       	| /v1/requests/{request_id}         	| OAuth                  	| request (privileged)                            	| requests.updateByID               	| - request_id - JavaScript object with parameters - callback function                                      	|
| DELETE      	| /v1/requests/{request_id}         	| OAuth                  	| request (privileged)                            	| requests.deleteByID               	| - request_id - callback function                                                                          	|
| GET         	| /v1/requests/{request_id}/map     	| OAuth                  	| request (privileged)                            	| requests.getMapByID               	| - request_id - callback function                                                                          	|
| GET         	| /v1/requests/{request_id}/receipt 	| OAuth                  	| request_receipt (privileged)                    	| requests.getReceiptByID           	| - request_id - callback function                                                                          	|
| GET         	| /v1/places/{place_id}             	| OAuth                  	| places                                          	| - places.getHome - places.getWork 	| - callback function                                                                                       	|
| PUT         	| /v1/places/{place_id}             	| OAuth                  	| places                                          	| places.updateByID                 	| - place_id (home or work) - address - callback function                                                   	|
| GET         	| v1/payment-methods                	| OAuth                  	| request (privileged)                            	| payment.getMethods                	| - callback                                                                                                	|
| POST        	| /v1/reminders                     	| server_token           	|                                                 	| reminders.create                  	| - JavaScript object with parameters - callback function                                                   	|
| GET         	| /v1/reminders/{reminder_id}       	| server_token           	|                                                 	| reminders.getByID                 	| - reminder_id - callback function                                                                         	|
| PATCH       	| /v1/reminders/{reminder_id}       	| server_token           	|                                                 	| reminders.updateByID              	| - reminder_id - JavaScript object with parameters - callback function                                     	|
| DELETE      	| /v1/reminders/{reminder_id}       	| server_token           	|                                                 	| reminders.deleteByID              	| - reminder_id - callback function                                                                         	|

TODOs
------------
- [ ] Add translations via 'Accept-Language'
- [ ] Advance Sandbox implementation
- [ ] Implement rate limit status
- [ ] Leverage Surge Pricing responses
- [ ] Checks for scopes
- [ ] Checks for auth methods
- [ ] Leverage Webhooks

Installation
------------

```sh
npm install node-uber
```

Test
------------
Run all existing test using script ``test/allTests.js``. This tests include linting, code coverage, and unit tests.

```sh
npm test
```

Usage
-----
```javascript
var Uber = require('node-uber');

var uber = new Uber({
  client_id: 'CLIENT_ID',
  client_secret: 'CLIENT_SECRET',
  server_token: 'SERVER_TOKEN',
  redirect_uri: 'REDIRECT URL',
  name: 'APP_NAME'
});
```

Resources / Endpoints
---------------------

### Authorization (OAuth 2.0)
#### Authrorize Url
After getting the authorize url, the user will be redirected to the redirect url with authorization code used in the next function.
```javascript
uber.getAuthorizeUrl(params);
```

##### Params
* scope (array)

_NOTE: `history` scope is still not working_

##### Example
```javascript
uber.getAuthorizeUrl(['profile']);
```
#### Authorization
Used to convert authorization code or refresh token into access token.
```javascript
uber.authorization(params, callback);
```
##### Params
* authorization_code OR
* refresh_token

##### Example
```javascript
uber.authorization({ refresh_token: 'REFRESH_TOKEN' },
  function (err, access_token, refresh_token) {
    if (err) console.error(err);
    else {
      console.log(access_token);
      console.log(refresh_token);
    }
  });

```

### Product
#### Types
```javascript
uber.products.list(params, callback);
```
##### Params
* latitude
* longitude

##### Example
```javascript
uber.products.list({ latitude: 3.1357, longitude: 101.6880 }, function (err, res) {
  if (err) console.error(err);
  else console.log(res);
});
```


### Estimates
#### Price
```javascript
uber.estimates.price(params, callback);
```
##### Params
* start_latitude
* start_longitude
* end_latitude
* end_longitude

##### Example
```javascript
uber.estimates.price({
  start_latitude: 3.1357, start_longitude: 101.6880,
  end_latitude: 3.0833, end_longitude: 101.6500
}, function (err, res) {
  if (err) console.error(err);
  else console.log(res);
});
```

#### Time
```javascript
uber.estimates.time(params, callback);
```
##### Params
* start_latitude
* start_longitude
* customer_uuid (not implemented)
* product_id (not implemented)

##### Example
```javascript
uber.estimates.time({
  start_latitude: 3.1357, start_longitude: 101.6880
}, function (err, res) {
  if (err) console.error(err);
  else console.log(res);
});
```

### User
#### Activity
```javascript
uber.user.activity(params, callback);
```

##### Params
* access_token
* offset (defautls to 0)
* limit (defaults to 5, maximum 50)

##### Example
```javascript
uber.user.activity({
  access_token: 'ACCESS_TOKEN'
}, function (err, res) {
  if (err) console.log(err);
  else console.log(res);
});
```

#### Profile
```javascript
uber.user.profile(params, callback);
```

##### Params (optional)
* access_token

_If this params is unable, the object will try to retrieve `access_token` inside_

##### Example
```javascript
uber.user.profile(params, function (err, res) {
  if (err) console.log(err);
  else console.log(res);
});
```

### Places
The `/places/{place_id}` endpoint provides access to predefined addresses for the current user. Must have authorization with `places` scope.

Right now, only home and work `place_id` is supported by the Uber API.



#### Home
```javascript
uber.places.home(callback);
```

##### Example
```javascript
uber.places.home(function(err, res) {
  if (err) {
    console.log(err);
  } else {
    console.log(res);
  }
});
```

#### Work
```javascript
uber.places.work(callback);
```

##### Example
```javascript
uber.places.work(function(err, res) {
  if (err) {
    console.log(err);
  } else {
    console.log(res);
  }
});
```

### Payment-Methods
The `/payment-methods` endpoint allows retrieving the list of the user’s available payment methods. These can be leveraged in order to supply a payment_method_id to the POST /v1/requests endpoint. Must have authorization with `request` scope.

```javascript
uber.payment.methods(callback);
```

#### Example
```javascript
uber.payment.methods(function(err, res) {
  if (err) {
    console.log(err);
  } else {
    console.log(res);
  }
});
```
