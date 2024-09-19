---
tags:
  - cypress
  - tests
---

[🏡 Home](../index.md) > [Learning](index.md) > 🌳 Cypress

# 🌳 Cypress

## Fundamental & basics

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


## Good practices
- Test cases names should be readable as a sentence: "it should display pending tasks" for example