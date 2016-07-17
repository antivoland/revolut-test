# Test assignment

## Abstract

Design and implement a RESTful API (including data model and the backing implementation) for money transfers between internal users/accounts.

Explicit requirements:

1. keep it simple and to the point (e.g. no need to implement any authentication, assume the APi is invoked by another internal system/service)
2. use whatever frameworks/libraries you like (except Spring, sorry!) but don't forget about the requirement #1
3. the datastore should run in­memory for the sake of this test
4. the final result should be executable as a standalone program (should not require a pre­installed container/server)
5. demonstrate with tests that the API works as expected

Implicit requirements:

1. the code produced by you is expected to be of good quality.
2. there are no detailed requirements, use common sense.

Please put your work on github or bitbucket.

## Solution

Seems that main problem of this test assignment is how to make atomic transfers.

First section of our API will be about wallet management:

- method `PUT /api/dev/wallets/{id}` allows us to create wallet with some initial balance in specified currency (in my implementation I will use [ISO 4217](http://www.iso.org/iso/home/standards/currency_codes.htm) three-letter codes)
- method `GET /api/dev/wallets/{id}` provides wallet details such as currency and balance

Assume we can transfer funds between wallets in the same currency without any fees. Otherwise we should use some currency conversion service (let's use [fixer API](http://fixer.io) for instance) and we also may want to specify some conversion fee.

So let's define some endpoints to handle wallet-to-wallet transfers:

- method `PUT /api/dev/transfers/{id}` initiates transfer
- method `GET /api/dev/transfers/{id}` provides some transfer-specific information (status, timestamp etc.)