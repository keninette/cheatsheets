---
tags:
  - cypress
  - tests
---

[🏡 Home](../index.md) > [Learning](index.md) > 🌳 Cypress

# 🌳 Cypress

## Fundamental & basics

### Use cypress

- `npx cypress open`: to run it from a browser.
- `npx cypress run`: to run it from CLI.

### Implicit & explicit assertions
Some commands in Cypress (such as `visit()` or `contains()`) have implicit expectations, which means that we expect them to succeed before going to the next command.
If the `visit()` command fails because there's a typo in the url, the test will fail and won't go any further.

Explicit expectations are the one we explicitely check (such as `should('exist')` for example).

### `get()` vs `find()`

Let's say we have this code (and forget about all the good practices it doesn't implement, it's for the example's sake): 

```html
<section id="products">
    <img src="./product-banner.png" alt="Our products" />
    ...
</section>
```

We want to make sure that the img banner inside our section is indeed visible:

```js
// This code is the most simple, it will work
cy.get('section#products > img').should('be.visible');

// This code will work too : find will "look inside" the elements we got with the get() method
cy.get('section#products').find('img').should('be.visible');

// This code will not seem like it, but it won't work the way we intend to: the second get() will not look "inside" the element we got in the first get(), but inside the DOM in general
// This code will actually look in the DOM for the section, then look in the DOM for the img. Both calls are not related.
cy.get('section#products').get('img').should('be.visible');
```

### The 4 seconds rule

There is a secret rule that is applied every time you make an assertion in Cypress : the 4 seconds rule. Basically, it means that if Cypress did not manage to do what you asked in the fiest place, it will retry for 4 seconds. If it manages to do it during this time, the assertion will pass. If not, an error mentionning a timeout will be displayed.

It applies to any assertion, even the simplest ones like `cy.get('section#products')`.

### `then` & `should`
There are 2 alternative ways of writing this code: 
```js
// Classical syntax
cy.get('section#products').should('have.attr', 'class').and('match', /invalid/);

// Alternative #1
cy.get('section#products').should(($section) => {
  expect($section).attr('class').to.contain('invalid');
});

// Alternative #2
cy.get('section#products').then(($section) => {
  expect($section).attr('class').to.contain('invalid');
});
```

In both alternatives, the `$section` argument is no longer a Cypress object, but a HTML element. This means you must interact with it in a more classical test writing syntax, you can't use Cypress methods anymore.
This can be useful if you need to override the 4 seconds rule.

Both syntaxes are equivalent, but prefer `should` to `then` since it seems to behave more consistently.


### Chaining & changing subject

Take the following code:

```js
cy.get('section#products').should('have.attr', 'class').and('match', /invalid/);
```

Sometimes (but not always), when chaining multiple methods, the subject on which the next method applies is different than the one we started with.
It depends on the method and is usually quite logic.

In our example, we first work with the section that was retrieved. Then we check its class. The `and` method is then applied to the class, not to the section we were using before.

### Tests hooks

There are 4 tests hooks to remember : 

- `before`: any code in there will be executed once, before running the first test.
- `beforeAll`: any code in there will be executed before every test.
- `afterAll`: any code in there will be executed after every test.
- `after`: any code in there will be executed once, after running the last test.

### Commands (and conquer) & queries

Commands & queries are tools to import into Cypress core some actions you need to perform throughout your whole project (or a majority of files).

Commands are re-usable shortcuts for complex command chains.
Queries are synchronous, chainable retriable commands.

Some relevant examples might be:
- A command to login to the app before running tests.
- A command to mock fetching current user data.
- A command to mock any API call, which could reflect your API structure.
- A query to get an element by its data-cy attribute, that would return the element

### Tasks
Tasks are functions that can be defined in Cypress config file & then called inside tests or tests hooks. They are very useful if you need to run code that cannot be executed in a browser.
They are useful to fake an third party API or a browser API.

```js
// In your app code
window.navigator.geolocation.getCurrentPosition(() => {});

// In your Cypress test
cy.visit('/').then((window) => {
  cy.stub(window.navigator.geolocation, 'getCurrentPosition').as('getUserPosition')
})
cy.get('@getUserPosition').should('have.been.called');
```

### Stubs
A stub is a replacement for an existing function/method. They're used for evaluating & controlling function calls.

### Spies
A spy is a Cypress tool that spies (with its little eye) a call of a specific function. It is useful when you want to make sure this function was indeed called, but when you don't need to fake its functioning (therefore when you don't need a stub)

### Manipulating the clock
Manipulating the clock allows us to fake the passing of time, because sometimes things happen after a delay and we don't want our test to actually wait.

Example:
```js
cy.clock();
cy.tick(2000); // Fake a 2 seconds delay
```

### Interceptors
Interceptors are a tool that cypress provides to intercept certain requests made during the tests. They're useful for:
- Making sure the call was made.
- Blocking the request by mocking its return value.
- You need to test a behaviour (for example the display of a success or error message), but don't need to test what's under the hood.

> 🚧 Intercepting will not necessarily block the request, to actually block it, you need to mock the result.

Example : 
```js
// Either you don't need to block the call, just intercept it.
cy.intercept('POST', 'https://newsletter.fnatic.com?*').as('subscribeNewsletter');
cy.get('@subscribeNewsletter').should('have.been.called');

// Or you want to block the call: mock the response
cy.intercept('POST', 'https://newsletter.fnatic.com?*', {status: 201}).as('subscribeNewsletter');

// In any case, if you need to wait for the response:
cy.wait('@subscribeNewsletter');
```

## Good practices
- Test cases names should be readable as a sentence: "it should display pending tasks" for example.
- Test cases should be isolated, they should not depend on the previous test & should always have the same starting point (reset database for example).
- Fixtures should make sense and be reusable throughout tests.
- The clock should be used to avoid actually waiting for our delays to expire, we should not actually wait.
