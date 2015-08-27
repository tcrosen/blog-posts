> This article explains how to use unit tests to plan & build services.

Let's build a service that makes API calls.  We don't want to burden ourselves with too many technical references, so keep it simple.
In simple terms, what do you want the service to do?

In this case, I want it to make some simple API calls to retrieve various datasets:

```js
describe('Service: Vehicles') => () {
  it('should fetch a list of models from the API') => () {
    // expect('this test to be written')
  }
  
  it('should fetch a list of trims from the API') => () {
    // expect('this test to be written')
  }
  
  // Continue adding tests for everything you can think of ...
}
```
