---
title:
description:
---

# Testing

Prior to deploying your Auth0 implementation to Production, we recommend running unit and integration tests to ensure that everything is working as expected. When running such tests, we recommend the following:

* Mock the Auth0 APIs. We do not permit unit/integration tests against the Auth0 APIs, and such activity may lead to triggering rate limits...
* 

Hi team, one reason for the recent ES-related issues (eg: ghost users) has been that some customers do unit/integration tests against Auth0 APIs. We forbid this and recommend them to mock the Auth0 APIs. I couldn't find any docs that recommends this. If there isn't a doc already, can we create one? (Please note that this is different from load testing)
Some points we should ideally cover:
1. Creating/deleting users at a high rate during a short period time is not recommended
2. They should mock the Auth0 APIs for their tests
3. Not adhering to above might cause them being rate-limited and unintended side-effects (and can probably be a violation of our ToS as well)