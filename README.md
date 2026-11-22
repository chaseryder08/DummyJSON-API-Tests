# DummyJSON API Tests

A Postman collection that logs in to DummyJSON (a free practice API), saves the login token, and uses it to make a few API calls. Each request has tests that check the response.

## Why I built this

In my support work I tested REST API calls and token-based login using Postman. This collection shows that same kind of work in a public project anyone can run.

## What it tests

1. **Login** - Sends a username and password. A script saves the returned access token (a JWT) to the environment so the other requests can use it.
2. **Get current user** - Uses the token to ask "who am I logged in as?" Checks for a 200 status, that an email field exists, and that the username is correct.
3. **Get current user - no token** - Same request with no token. Checks that the API rejects it with a 401 Unauthorized.
4. **Add Product** - Sends a new product with the token. Checks for a 201 Created status, that the response has an id, and that the title matches what was sent.

## How to run it

1. Download both JSON files in this repo.
2. In Postman, click Import and add both files.
3. Select the "Postman API project" environment in the top-right corner.
4. Run the collection. Login must run first, since it creates the token.

The login uses DummyJSON's public demo account (username `emilys`), so no signup is needed. The token in the environment is left blank on purpose - it fills in when Login runs.

## What I found

When I ran each request by itself, all the tests passed. When I ran the whole collection, the "no token" test failed: it got a 200 instead of a 401. The login response was also setting the token as a cookie, and Postman was sending that cookie on later requests. So the "no token" request was still logged in. I fixed it by turning on "Disable cookie jar" for the Login request, so the cookie is never saved.
