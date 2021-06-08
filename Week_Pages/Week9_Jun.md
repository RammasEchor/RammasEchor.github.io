# Week 9 - Building something from scratch (4)

## 01 Jun 2021 - 07 Jun 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## Mocha

Mocha is a test framework that we used for the backend of our project. Mocha has a very simple interface that lets you *read* what is it doing. For example, what do you think this piece of code do?

    var assert = require('assert');
    describe('Array', function() {
        describe('#indexOf()', function() {
            it('should return -1 when the value is not present', function() {
                assert.equal([1, 2, 3].indexOf(4), -1);
            });
        });
    });

That's right, it checks the function of `Array` called `indexOf()` so that this function return `-1` when a value is not present. Mocha presents us two main functions:

- `describe()`: It may be nested, and it is used for separating tests into topics. You may have this:

        describe('Store', function() {
            describe('when closed', function() {
                // Tests
            });
            describe('when open', function() {
                // Tests
            });
        });

    and the test messages will look like this:

        Store:
            when closed:
                // Test result
            when open:
                // Test result

    Describe is incredibly useful for checking the tests with our own human eyes.

- `it()`: It is where you put the tests. If we take the previous example and extend it,

        describe('Store', function() {
                    describe('when closed', function() {
                        it('should not sell', function() {
                            const store = new Store({
                                'Cheese': '10',
                                'Milk': '12'
                            }, 'closed' );
                            const item = store.sell('Cheese');
                            assert( store.has('Cheese') );
                        });
                    });
                    describe('when open', function() {
                        it('should sell', function() {
                            const store = new Store({
                                'Cheese': '10',
                                'Milk': '12'
                            }, 'open' );
                            const item = store.sell('Cheese');
                            assert( !store.has('Cheese') );
                            assert( item === 'Cheese' );
                        });
                    });
                });

    we start to see how we may create tests with descriptions. The output of the succesful tests would be:

        Store:
            when closed:
                ✔ should not sell
            when open:
                ✔ should sell

## Jest

I learned how to use the libraries for testing that already came in the project (Jest). The idea is to take something already integrated, and relatively easy to use.

Jest uses the directory *_tests_* to search for tests called *something.test.js*.

Tests are searched in:

- Files with .js suffix in __tests__ folders.

- Files with .test.js suffix.

- Files with .spec.js suffix.

I learned how to test the frontend via a method that renders a page, and then searched for elements with the specified name. For example, we search for a button with the name "*Submit*". Then, we may check if it is active, or inactive.
