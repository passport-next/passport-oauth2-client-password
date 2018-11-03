# passport-oauth2-client-password

[![NPM version](https://img.shields.io/npm/v/@passport-next/passport-oauth2-client-password.svg)](https://www.npmjs.com/package/@passport-next/passport-oauth2-client-password)
[![Build Status](https://travis-ci.org/passport-next/passport-oauth2-client-password.svg?branch=master)](https://travis-ci.org/passport-next/passport-oauth2-client-password)
[![Coverage Status](https://coveralls.io/repos/github/passport-next/passport-oauth2-client-password/badge.svg?branch=master)](https://coveralls.io/github/passport-next/passport-oauth2-client-password?branch=master)
[![Maintainability](https://api.codeclimate.com/v1/badges/998ae49d9d0c5d96003e/maintainability)](https://codeclimate.com/github/passport-next/passport-oauth2-client-password/maintainability)
[![Dependencies](https://david-dm.org/passport-next/passport-oauth2-client-password.png)](https://david-dm.org/passport-next/passport-oauth2-client-password)
<!--[![SAST](https://gitlab.com/passport-next/passport-oauth2-client-password/badges/master/build.svg)](https://gitlab.com/passport-next/passport-oauth2-client-password/badges/master/build.svg)-->


OAuth 2.0 client password authentication strategy for [Passport](https://github.com/passport-next/passport).

This module lets you authenticate requests containing client credentials in the
request body, as [defined](http://tools.ietf.org/html/draft-ietf-oauth-v2-27#section-2.3.1)
by the OAuth 2.0 specification.  These credentials are typically used protect
the token endpoint and used as an alternative to HTTP Basic authentication.

## Install

    $ npm install @passport-next/passport-oauth2-client-password

## Usage

#### Configure Strategy

The OAuth 2.0 client password authentication strategy authenticates clients
using a client ID and client secret.  The strategy requires a `verify` callback,
which accepts those credentials and calls `done` providing a client.

    passport.use(new ClientPasswordStrategy(
      function(clientId, clientSecret, done) {
        Clients.findOne({ clientId: clientId }, function (err, client) {
          if (err) { return done(err); }
          if (!client) { return done(null, false); }
          if (client.clientSecret != clientSecret) { return done(null, false); }
          return done(null, client);
        });
      }
    ));

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'oauth2-client-password'`
strategy, to authenticate requests.  This strategy is typically used in
combination with HTTP Basic authentication (as provided by [passport-http](https://github.com/passport-next/passport-http)),
allowing clients to include credentials in the request body.

For example, as route middleware in an [Express](http://expressjs.com/)
application, using [OAuth2orize](https://github.com/passport-next/oauth2orize)
middleware to implement the token endpoint:

    app.get('/profile', 
      passport.authenticate(['basic', 'oauth2-client-password'], { session: false }),
      oauth2orize.token());

## Examples

The [example](https://github.com/passport-next/oauth2orize/tree/master/examples/express2)
included with [OAuth2orize](https://github.com/passport-next/oauth2orize)
demonstrates how to implement a complete OAuth 2.0 authorization server.
`ClientPasswordStrategy` is used to authenticate clients as they request access
tokens from the token endpoint.

## Tests

    $ npm install --dev
    $ npm test

