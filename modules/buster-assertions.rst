.. default-domain:: js
.. highlight:: javascript
.. _buster-assertions:

=================
buster-assertions
=================

Version:
    0.10.1 (2012-04-26)
Module:
    ``require("buster-assertions");``
In browsers:
    ``buster.assertions;``

A collection of assertions to be used with a unit testing framework.
**buster-assertions** works well with any CommonJS compliant testing framework
out of the box, and can easily be configured to work with most any testing
framework. See also :ref:`expectations` if you like the alternative API
(``expect(thing).toBe*``).

**buster-assertions** contains lots of assertions. We
strongly believe that high-level assertions are essential in the
interest of producing clear and intent-revealing tests, and they also
give you to-the-point failure messages.


Assertions and refutations
==========================

Unlike most assertion libraries, **buster-assertion** does not have
``assert.notXyz`` assertions to refute some fact. Instead, it has
*refutations*, heavily inspired by Ruby's `minitest
<http://bfts.rubyforge.org/minitest/>`_::

    var assert = buster.assertions.assert;
    var refute = buster.assertions.refute;

    assert.equals(42, 42);
    refute.equals(42, 43);

Refutations help express "assert not ..." style verification in a much clearer
way. It also brings with it a nice consistency in that any ``assert.xyz``
always has a corresponding ``refute.xyz`` that does the opposite check.

.. function:: assert

    ::

        assert(actual[, message]);

    Fails if ``actual`` is falsy (``0``, ``""``, ``null``, ``undefined``,
    ``NaN``). Fails with either the provided message or "Expected null to be
    truthy". This behavior differs from all other assertions, which prepend
    the optional message argument.

    ::

        assert({ not: "Falsy" }, "This will pass");
        assert(null, "This will fail"); // Fails with custom message
        assert(null); // Fails
        assert(34);   // Passes

.. function:: refute

    ::

        refute(actual[, message])

    Fails if ``actual`` is truthy. Fails with either the provided message or
    "Expected null to be falsy". This behavior differs from all other
    refutations, which prepend the optional message argument.

    ::

        refute({ not: "Falsy" }, "This will fail"); // Fails with custom message
        refute(null, "This will pass");
        refute(null); // Passes
        refute(34);   // Fails


Predefined Assertions
=====================

The following assertions can be used with ``assert`` and ``refute``.
They are described for ``assert``, but the corresponding failure messages for ``refute``
are also mentioned. For ``refute`` the behaviour is exactly opposed.

All assertions support an optional ``message`` argument, which is prepended to the
failure message.

*Overview:*

- :func:`same`
- :func:`equals`
- :func:`greater`
- :func:`less`
- :func:`defined`
- :func:`isNull`
- :func:`match`
- :func:`isObject`
- :func:`isFunction`
- :func:`isTrue`
- :func:`isFalse`
- :func:`isString`
- :func:`isBoolean`
- :func:`isNumber`
- :func:`isNaN`
- :func:`isArray`
- :func:`isArrayLike`
- :func:`exception`
- :func:`near`
- :func:`hasPrototype`
- :func:`contains`
- :func:`tagName`
- :func:`className`

.. function:: same

    ::

        assert.same(actual, expected[, message])

    Fails if ``actual`` **is not** the same object (``===``) as ``expected``.
    To compare similar objects, such as ``{ name: "Chris", id: 42 }`` and ``{
    id: 42, name: "Chris" }`` (not the same instance), see
    :func:`equals`.

    ::

        var obj = { id: 42, name: "Chris" };
        assert.same(obj, obj);                       // Passes
        assert.same(obj, { id: 42, name: "Chris" }); // Fails

    **Messages**

    ::

        assert.same.message = "${0} expected to be the same object as ${1}";
        refute.same.message = "${0} expected not to be the same object as ${1}";

    ``${0}``:
        The actual object
    ``${1}``:
        The expected object


.. function:: equals

    ::

        assert.equals(actual, expected[, message])

    Compares ``actual`` to ``expected`` property by property. If the property
    count does not match, or if any of ``actual``'s properties does not match
    the corresponding property in ``expected``, the assertion fails. Object
    properties are verified recursively.

    If ``actual`` is ``null`` or ``undefined``, an exact match is required.
    Date objects are compared by their ``getTime`` method. Regular expressions
    are compared by their string representations. Primitives are compared using
    ``==``, i.e., with coercion.

    ``equals`` passes when comparing an ``arguments`` object to an array if the
    both contain the same elements.

    ::

        assert.equals({ name: "Professor Chaos" }, { name: "Professor Chaos" }); // Passes
        assert.equals({ name: "Professor Chaos" }, { name: "Dr Evil" });         // Fails

    **Messages**

    ::

        assert.equals.message = "${0} expected to be equal to ${1}";
        refute.equals.message = "${0} expected not to be equal to ${1}";

    ``${0}``:
        The actual object
    ``${1}``:
        The expected object


.. function:: greater

    ::

        assert.greater(actual, expected[, message])

    Fails if ``actual`` is equal to or less than ``expected``.

    ::

        assert.greater(2, 1); // Passes
	assert.greater(1, 1); // Fails
        assert.greater(1, 2); // Fails

    **Messages**

    ::

        assert.greater.message = "Expected ${0} to be greater than ${1}";
        refute.greater.message = "Expected ${0} to be less than or equal to ${1}";

    ``${0}``:
        The actual object
    ``${1}``:
        The expected object


.. function:: less

    ::

        assert.less(actual, expected[, message])

    Fails if ``actual`` is equal to or greater than ``expected``.

    ::

        assert.less(1, 2); // Passes
	assert.less(1, 1); // Fails
        assert.less(2, 1); // Fails

    **Messages**

    ::

        assert.less.message = "Expected ${0} to be less than ${1}";
        refute.less.message = "Expected ${0} to be greater than or equal to ${1}";

    ``${0}``:
        The actual object
    ``${1}``:
        The expected object


.. function:: defined

    ::

        assert.defined(object[, message])

    Fails if ``object`` is ``undefined``.

    ::

        var a;
        assert.defined({});  // Passes
        assert.defined(a); // Fails

    **Messages**

    ::

        assert.defined.message = "Expected to be defined";
        refute.defined.message = "typeof ${0} (${1}) expected to be undefined";

    ``${0}``:
        The actual object
    ``${1}``:
        ``typeof object``



.. function:: isNull

    ::

        assert.isNull(object[, message])

    Fails if ``object`` is not ``null``.

    ::

        assert.isNull(null, "This will pass");
        assert.isNull({}, "This will fail");
        assert.isNull(null); // Passes
        assert.isNull({});   // Fails

    **Messages**

    ::

        assert.isNull.message = "Expected ${0} to be null";
        refute.isNull.message = "Expected not to be null";

    ``${0}``:
        The actual object


.. function:: match

    ::

        assert.match(actual, matcher[, message])

    Fails if ``matcher`` is not a partial match for ``actual``. Accepts a wide
    range of input combinations.  Note that ``assert.match`` is not symmetric -
    in some cases ``assert.match(a, b)`` may pass while ``assert.match(b, a)``
    fails.

    **String matcher**

    In its simplest form, ``assert.match`` performs a case insensitive
    substring match. When the matcher is a string, the ``actual`` object is
    converted to a string, and the assertion passes if ``actual`` is a
    case-insensitive substring of ``expected`` as a string.

    ::

        assert.match("Give me something", "Give");                           // Passes
        assert.match("Give me something", "sumptn");                         // Fails
        assert.match({ toString: function () { return "yeah"; } }, "Yeah!"); // Passes

    The last example is not symmetric. When the matcher is a string, the actual
    value is coerced to a string - in this case using ``toString``. Changing
    the order of the arguments would cause the matcher to be an object, in
    which case different rules apply (see below).

    **Boolean matcher**

    Performs a strict (i.e. ``===``) match with the object. So, only ``true``
    matches ``true``, and only ``false`` matches ``false``.

    **Regular expression matcher**

    When the matcher is a regular expression, the assertion will pass if
    ``expected.test(actual)`` is true. ``assert.match`` is written in a generic
    way, so any object with a ``test`` method will be used as a matcher this
    way.

    ::

        assert.match("Give me something", /^[a-z\s]$/i); // Passes
        assert.match("Give me something", /[0-9]/); // Fails
        assert.match({ toString: function () { return "yeah!"; } }, /yeah/); // Passes
        assert.match(234, /[a-z]/); // Fails

    **Number matcher**

    When the matcher is a number, the assertion will pass if ``matcher ==
    actual``.

    ::

        assert.match("123", 123); // Passes
        assert.match("Give me something", 425); // Fails
        assert.match({ toString: function () { return "42"; } }, 42); // Passes
        assert.match(234, 1234); // Fails


    **Function matcher**

    When the matcher is a function, it is called with ``actual`` as its only
    argument. The assertion will pass if the function returns ``true``. A
    strict match is performed against the return value, so a boolean ``true``
    is required, truthy is not enough.

    ::

        // Passes
        assert.match("123", function (exp) {
            return exp == "123";
        });

        // Fails
        assert.match("Give me something", function () {
            return "ok";
        });

        // Passes
        assert.match({
            toString: function () {
                return "42";
            }
        }, function () { return true; });

        // Fails
        assert.match(234, function () {});

    **Object matcher**

    As mentioned above, if an object matcher defines a ``test`` method the
    assertion will pass if ``matcher.test(actual)`` returns truthy. If the
    object does not have a ``test`` method, a recursive match is performed. If
    all properties of ``matcher`` matches corresponding properties in
    ``actual``, the assertion passes. Note that the object matcher does not
    care if the number of properties in the two objects are the same - only if
    all properties in the matcher recursively "matches" ones in the actual
    object.

    ::

        // Passes
        assert.match("123", {
            test: function (arg) {
                return arg == 123;
            }
        });

        // Fails
        assert.match({}, { prop: 42 });

        // Passes
        assert.match({
            name: "Chris",
            profession: "Programmer"
        }, {
            name: "Chris"
        });

        // Fails
        assert.match(234, {
            name: "Chris"
        });


    **DOM elements**

    ``assert.match`` can be very helpful when asserting on DOM elements,
    because it allows you to compare several properties with one assertion::

        var el = document.getElementById("myEl");

        assert.match(el, {
            tagName: "h2",
            className: "item",
            innerHTML: "Howdy"
        });

    **Messages**

    ::

        assert.match.exceptionMessage = "${0}";
        refute.match.exceptionMessage = "${0}";

    Used when the matcher function throws an exception. This happens if the
    matcher is not any of the accepted types, for instance, a boolean.

    ``${0}``
        Message from exception thrown by matcher function.

    ::

        assert.match.message = "${0} expected to match ${1}";
        refute.match.message = "${0} expected not to match ${1}";

    ``${0}``:
        The actual object
    ``${1}``:
      The expected object


.. function:: isObject

    ::

        assert.isObject(object[, message])

    Fails if ``object`` is not an object or if it is ``null``.

    ::

        assert.isObject({});             // Passes
        assert.isObject(42);             // Fails
        assert.isObject([1, 2, 3]);      // Passes
        assert.isObject(function () {}); // Fails

    **Messages**

    ::

        assert.isObject.message = "${0} (${1}) expected to be object and not null";
        refute.isObject.message = "${0} (${1}) expected not to be object and not null";

    ``${0}``:
        The actual object
    ``${1}``:
      ``typeof object``


.. function:: isFunction

    ::

        assert.isFunction(actual[, message])

    Fails if ``actual`` is not a function.

    ::

        assert.isFunction({});             // Fails
        assert.isFunction(42);             // Fails
        assert.isFunction(function () {}); // Passes

    **Messages**

    ::

        assert.isFunction.message = "${0} (${1}) expected to be function";
        refute.isFunction.message = "${0} (${1}) expected not to be function";

    ``${0}``:
        The actual value
    ``${1}``
        ``typeof actual value``


.. function:: isTrue

    ::

        assert.isTrue(actual[, message])

    Fails if ``actual`` is not ``true``.

    ::

        assert.isTrue("2" == 2);  // Passes
        assert.isTrue("2" === 2); // Fails

    **Messages**

    ::

        assert.isTrue.message = "Expected ${0} to be true";
        refute.isTrue.message = "Expected ${0} to not be true";

    ``${0}``:
        The actual value


.. function:: isFalse

    ::

        assert.isFalse(actual[, message])

    Fails if ``actual`` is not ``false``.

    ::

        assert.isFalse("2" === 2); // Passes
        assert.isFalse("2" == 2);  // Fails

    **Messages**

    ::

        assert.isFalse.message = "Expected ${0} to be false";
        refute.isFalse.message = "Expected ${0} to not be false";

    ``${0}``:
        The actual value


.. function:: isString

    ::

        assert.isString(actual[, message])

    Fails if the type of ``actual`` is not ``"string"``.

    ::

        assert.isString("2"); // Passes
        assert.isString(2);   // Fails

    **Messages**

    ::

        assert.isString.message = "Expected ${0} (${1}) to be string";
        refute.isString.message = "Expected ${0} not to be string";

    ``${0}``:
        The actual value
    ``${1}``:
        The type of the actual value


.. function:: isBoolean

    ::

        assert.isBoolean(actual[, message])

    Fails if the type of ``actual`` is not ``"boolean"``.

    ::

        assert.isBoolean(true);   // Passes
        assert.isBoolean(2 < 2);  // Passes
        assert.isBoolean("true"); // Fails

    **Messages**

    ::

        assert.isBoolean.message = "Expected ${0} (${1}) to be boolean";
        refute.isBoolean.message = "Expected ${0} not to be boolean";

    ``${0}``:
        The actual value
    ``${1}``:
        The type of the actual value


.. function:: isNumber

    ::

        assert.isNumber(actual[, message])

    Fails if the type of ``actual`` is not ``"number"`` or is ``NaN``.

    ::

        assert.isNumber(12);   // Passes
        assert.isNumber("12"); // Fails
        assert.isNumber(NaN);  // Fails

    **Messages**

    ::

        assert.isNumber.message = "Expected ${0} (${1}) to be a non-NaN number";
        refute.isNumber.message = "Expected ${0} to be NaN or another non-number value";

    ``${0}``:
        The actual value
    ``${1}``:
        The type of the actual value


.. function:: isNaN

    ::

        assert.isNaN(actual[, message])

    Fails if ``actual`` is not ``NaN``.
    Does not perform coercion in contrast to the standard javascript function ``isNaN``.

    ::

        assert.isNaN(NaN);           // Passes
        assert.isNaN("abc" / "def"); // Passes
        assert.isNaN(12);            // Fails
        assert.isNaN({});            // Fails, would pass for standard javascript function isNaN

    **Messages**

    ::

        assert.isNaN.message = "Expected ${0} to be NaN";
        refute.isNaN.message = "Expected not to be NaN";

    ``${0}``:
        The actual value


.. function:: isArray

    ::

        assert.isArray(actual[, message])

    Fails if the object type of ``actual`` is not ``Array``.
    
    ::

        assert.isArray([1, 2, 3]); // Passes
        assert.isArray({});        // Fails

    **Messages**

    ::

        assert.isArray.message = "Expected ${0} to be array";
        refute.isArray.message = "Expected ${0} not to be array";

    ``${0}``:
        The actual value


.. function:: isArrayLike

    ::

        assert.isArrayLike(actual[, message])

    Fails if none of the following conditions is fulfilled:

    - the object type of ``actual`` is ``Array``
    - ``actual`` is an ``arguments`` object
    - ``actual`` is an object providing a property ``length`` of type ``"number"`` and a property ``splice`` of type ``"function"``
    
    ::

        assert.isArrayLike([1, 2, 3]);                            // Passes
	assert.isArrayLike(arguments);                            // Passes
	assert.isArrayLike({ length: 0, splice: function() {} }); // Passes
        assert.isArrayLike({});                                   // Fails

    **Messages**

    ::

        assert.isArrayLike.message = "Expected ${0} to be array like";
        refute.isArrayLike.message = "Expected ${0} not to be array like";

    ``${0}``:
        The actual value


.. function:: exception

    ::

        assert.exception(callback[, type, message])

    Fails if ``callback`` does not throw an exception. If the optional ``type``
    is provided, the assertion fails if the callback either does not throw an
    exception, **or** if the exception is not of the given type (determined by
    its ``name`` property).

    ::

        // Passes
        assert.exception(function () {
            throw new Error("Ooops!");
        });

        // Fails
        assert.exception(function () {});

        // Passes
        assert.exception(function () {
            throw new TypeError("Ooops!");
        }, "TypeError");

        // Fails, wrong exception type
        assert.exception(function () {
            throw new Error("Aww");
        }, "TypeError");

        // Fails
        assert.exception(function () {}, "TypeError");

    **Messages**

    ::

        assert.exception.typeNoExceptionMessage = "Expected ${0} but no exception was thrown";
        assert.exception.message = "Expected exception";
        assert.exception.typeFailMessage = "Expected ${0} but threw ${1}, (${2})";

    ``${0}``:
        The expected exception type (if provided)
    ``${1}``:
        The type of exception thrown (if any)
    ``${2}``:
        The exception message

    ::

        refute.exception.message = "Expected not to throw but threw ${0}, (${1})";

    ``${0}``:
        The type of exception thrown (if any)
    ``${1}``:
        The exception message


.. function:: near

    ::

        assert.near(actual, expected, delta[, message])

    Fails if the difference between ``actual`` and ``expected`` is greater than ``delta``.

    ::

        assert.near(10.3, 10, 0.5); // Passes
        assert.near(10.5, 10, 0.5); // Passes
        assert.near(10.6, 10, 0.5); // Fails

    **Messages**

    ::

        assert.near.message = "Expected ${0} to be equal to ${1} +/- ${2}";
        refute.near.message = "Expected ${0} not to be equal to ${1} +/- ${2}";

    ``${0}``:
        The ``actual`` value 
    ``${1}``:
	The ``expected`` value
    ``${2}``:
	The ``delta`` value


.. function:: hasPrototype

    ::

        assert.hasPrototype(actual, prototype[, message])

    Fails if ``prototype`` does not exist in the prototype chain of ``actual``.

    ::

        assert.hasPrototype(function() {}, Function.prototype); // Passes
        assert.hasPrototype(function() {}, Object.prototype);   // Passes
        assert.hasPrototype({}, Function.prototype);            // Fails

    **Messages**

    ::

        assert.hasPrototype.message = "Expected ${0} to have ${1} on its prototype chain";
        refute.hasPrototype.message = "Expected ${0} not to have ${1} on its prototype chain";

    ``${0}``:
        The ``actual`` object 
    ``${1}``:
	The ``prototype`` object


.. function:: contains

    ::

        assert.contains(haystack, needle[, message])

    Fails if the array like object ``haystack`` does not contain the ``needle`` object.

    ::

        assert.contains([1, 2, 3], 2);   // Passes
        assert.contains([1, 2, 3], 4);   // Fails
        assert.contains([1, 2, 3], "2"); // Fails

    **Messages**

    ::

        assert.contains.message = "Expected [${0}] to contain ${1}";
        refute.contains.message = "Expected [${0}] not to contain ${1}";

    ``${0}``:
        The ``haystack`` object 
    ``${1}``:
	The ``needle`` object


.. function:: tagName

    ::

        assert.tagName(element, tagName[, message])

    Fails if the ``element`` either does not specify a ``tagName`` property, or
    if its value is not a case-insensitive match with the expected ``tagName``.
    Works with any object.

    ::

        assert.tagName(document.createElement("p"), "p"); // Passes
        assert.tagName(document.createElement("h2"), "H2"); // Passes
        assert.tagName(document.createElement("p"), "li");  // Fails

    **Messages**

    ::

        assert.tagName.noTagNameMessage = "Expected ${1} to have tagName property";
        assert.tagName.message = "Expected tagName to be ${0} but was ${1}";
        refute.tagName.noTagNameMessage = "Expected ${1} to have tagName property";

    ``${0}``:
        The expected ``tagName``
    ``${1}``:
        If the object does not have a ``tagName`` property, this is the object.
        Otherwise, it is the value of ``object.tagName``.


.. function:: className

    ::

        assert.className(element, className[, message])

    Fails if the ``element`` either does not specify a ``className`` property,
    or if its value is not a space-separated list of all class names in
    ``classNames``.

    ``classNames`` can be either a space-delimited string or an array of class
    names. Every class specified by ``classNames`` must be found in the
    object's ``className`` property for the assertion to pass, but order does
    not matter.

    ::

        var el = document.createElement("p");
        el.className = "feed item blog-post";

        assert.className(el, "item");           // Passes
        assert.className(el, "news");           // Fails
        assert.className(el, "blog-post feed"); // Passes
        assert.className(el, "feed items");     // Fails, "items" is not a match
        assert.className(el, ["item", "feed"]); // Passes

    **Messages**

    ::

        assert.className.noClassNameMessage = "Expected object to have className property";
        assert.className.message = "Expected object's className to include ${0} but was ${1}";
        refute.className.noClassNameMessage = "Expected object to have className property";
        refute.className.message = "Expected object's className to not include ${0} but was ${1}";

    ``${0}``:
        The expected ``classNames``
    ``${1}``:
      The value of the object's ``className`` property, if any. Otherwise, the
      object itself.


Custom assertions
=================

Custom, domain-specific assertions helps improve clarity and reveal intent in
tests. They also facilitate much better feedback when they fail. You can add
custom assertions that behave exactly like the built-in ones (i.e. with
counting, message formatting, expectations and more) by using the :func:`add`
method.


Overriding assertion messages
=============================

The default assertion messages can be overridden. The properties to overwrite
are listed with each assertion along with the arguments the string is fed.
Here's an example of providing a new assertion failure message for
:func:`assert.equals`::

    var assert = buster.assertions.assert;
    assert.equals.message = "I wanted ${0} == ${1}!"

    try {
        assert.equals(3, 4);
    } catch (e) {
        console.log(e.message);
    }

    // Prints:
    // "I wanted 3 == 4!"


Events
======

``buster.assertions`` is an :ref:`event-emitter`. Listen to events with
``on``::

    buster.assertions.on("failure", function (err) {
        console.log(err.message);
    });


``pass`` event
--------------

Signature::

    "pass", function () {}

Assertion passed. The callback is invoked with the assertion name, e.g.
``"equals"``, as its only argument. Note that this event is also emitted when
refutations pass.


``failure`` event
-----------------

Signature::

    "failure", function (error) {}

Assertion failed. The callback is invoked with an :class:`AssertionError`
object.


.. _stubs-and-spies:

Stubs and spies
===============

The default Buster.JS bundle comes with built-in spies, stubs and mocks
provided by `Sinon.JS <http://sinonjs.org>`_. The assertions are indisposable
when working with spies and stubs. However, note that these assertions are
technically provided by the integration package :ref:`buster-sinon`, *not*
**buster-assertions**. This only matters if you use this package stand-alone.

As for the normal assertions, the assertions for stubs and spies can be used with ``assert`` and ``refute``.
The description is for ``assert``, but the corresponding failure messages for ``refute`` are also mentioned.
For ``refute`` the behaviour is exactly opposed.

*Overview:*

- :func:`same`
- :func:`called`
- :func:`callOrder`
- :func:`calledOnce`
- :func:`calledTwice`
- :func:`calledThrice`
- :func:`calledOn`
- :func:`alwaysCalledOn`
- :func:`calledWith`
- :func:`alwaysCalledWith`
- :func:`calledOnceWith`
- :func:`calledWithExactly`
- :func:`alwaysCalledWithExactly`
- :func:`threw`
- :func:`alwaysThrew`


.. function:: called

    ::

        assert.called(spy)

    Fails if the ``spy`` has never been called.

    ::

        var spy = this.spy();

        assert.called(spy); // Fails

        spy();
        assert.called(spy); // Passes

        spy();
        assert.called(spy); // Passes

    **Messages**

    ::

        assert.called.message = "Expected ${0} to be called at least once but was never called";

    ``${0}``:
        The spy

    ::

        refute.called.message = "Expected ${0} to not be called but was called ${1}${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The number of calls as a string. Ex: "two times".
    ``${2}``:
        All calls formatted as a multi-line string.


.. function:: callOrder

    ::

        assert.callOrder(spy, spy2, ...)

    Fails if the spies were not called in the specified order.

    ::

        var spy1 = this.spy();
        var spy2 = this.spy();
        var spy3 = this.spy();

        spy1();
        spy2();
        spy3();

        assert.callOrder(spy1, spy3, spy2); // Fails
        assert.callOrder(spy1, spy2, spy3); // Passes

    **Messages**

    ::

        assert.callOrder.message = "Expected ${expected} to be called in order but were called as ${actual}";
        refute.callOrder.message = "Expected ${expected} not to be called in order";

    ``${expected}``:
        A string representation of the expected call order
    ``${actual}``:
        A string representation of the actual call order


.. function:: calledOnce

    ::

        assert.calledOnce(spy)

    Fails if the ``spy`` has never been called or if it was called more than once.

    ::

        var spy = this.spy();

        assert.calledOnce(spy); // Fails

        spy();
        assert.calledOnce(spy); // Passes

        spy();
        assert.calledOnce(spy); // Fails

    **Messages**

    ::

        assert.calledOnce.message = "Expected ${0} to be called once but was called ${1}${2}";
        refute.calledOnce.message = "Expected ${0} to not be called exactly once${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The number of calls, as a string. Ex: "two times"
    ``${2}``:
        The call log. All calls as a string. Each line is one call and includes
        passed arguments, returned value and more.


.. function:: calledTwice

    ::

        assert.calledTwice(spy)

    Only passes if the ``spy`` was called exactly two times.

    ::

        var spy = this.spy();

        assert.calledTwice(spy); // Fails

        spy();
        assert.calledTwice(spy); // Fails

        spy();
        assert.calledTwice(spy); // Passes

        spy();
        assert.calledTwice(spy); // Fails

    **Messages**

    ::

        assert.calledTwice.message = "Expected ${0} to be called twice but was called ${1}${2}";
        refute.calledTwice.message = "Expected ${0} to not be called exactly twice${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The number of calls, as a string. Ex: "two times"
    ``${2}``:
        The call log. All calls as a string. Each line is one call and includes
        passed arguments, returned value and more.


.. function:: calledThrice

    ::

        assert.calledThrice(spy)

    Only passes if the ``spy`` has been called exactly three times.

    ::

        var spy = this.spy();

        assert.calledThrice(spy); // Fails

        spy();
        assert.calledThrice(spy); // Fails

        spy();
        assert.calledThrice(spy); // Passes

        spy();
        assert.calledThrice(spy); // Fails

    **Messages**

    ::

        assert.calledThrice.message = "Expected ${0} to be called thrice but was called ${1}${2}";
        refute.calledThrice.message = "Expected ${0} to not be called exactly thrice${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The number of calls, as a string. Ex: "two times"
    ``${2}``:
        The call log. All calls as a string. Each line is one call and includes
        passed arguments, returned value and more.


.. function:: calledOn

    ::

        assert.calledOn(spy, obj)

    Passes if the ``spy`` was called at least once with ``obj`` as its ``this`` value.

    ::

        var spy = this.spy();
	var obj1 = {};
	var obj2 = {};
	var obj3 = {};

        spy.call(obj2);
        spy.call(obj3);

        assert.calledOn(spy, obj1); // Fails
        assert.calledOn(spy, obj2); // Passes
        assert.calledOn(spy, obj3); // Passes

    **Messages**

    ::

        assert.calledOn.message = "Expected ${0} to be called with ${1} as this but was called on ${2}";
        refute.calledOn.message = "Expected ${0} not to be called with ${1} as this";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The object ``obj`` which is expected to have been ``this`` at least once
    ``${2}``:
        List of objects which actually have been ``this``


.. function:: alwaysCalledOn

    ::

        assert.alwaysCalledOn(spy, obj)

    Passes if the ``spy`` was always called with ``obj`` as its ``this`` value.

    ::

        var spy1 = this.spy();
        var spy2 = this.spy();
	var obj1 = {};
	var obj2 = {};

        spy1.call(obj1);
        spy1.call(obj2);
        
        spy2.call(obj2);
        spy2.call(obj2);

        assert.alwaysCalledOn(spy1, obj1); // Fails
        assert.alwaysCalledOn(spy1, obj2); // Fails
        assert.alwaysCalledOn(spy2, obj1); // Fails
        assert.alwaysCalledOn(spy2, obj2); // Passes

    **Messages**

    ::

        assert.alwaysCalledOn.message = "Expected ${0} to always be called with ${1} as this but was called on ${2}";
        refute.alwaysCalledOn.message = "Expected ${0} not to always be called with ${1} as this";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The object ``obj`` which is expected always to have been ``this``
    ``${2}``:
        List of objects which actually have been ``this``


.. function:: calledWith

    ::

        assert.calledWith(spy, arg1, arg2, ...)

    Passes if the ``spy`` was called at least once with the specified arguments.
    Other arguments may have been passed after the specified ones.

    ::

        var spy = this.spy();
        var arr = [1, 2, 3];
        spy(12);
        spy(42, 13);
        spy("Hey", arr, 2);

        assert.calledWith(spy, 12);         // Passes
        assert.calledWith(spy, "Hey");      // Passes
        assert.calledWith(spy, "Hey", 12);  // Fails
        assert.calledWith(spy, "Hey", arr); // Passes

    **Messages**

    ::

        assert.calledWith.message = "Expected ${0} to be called with arguments ${1}${2}";
        refute.calledWith.message = "Expected ${0} not to be called with arguments ${1}${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The expected arguments
    ``${2}``:
        String representation of all calls.


.. function:: alwaysCalledWith

    ::

        assert.alwaysCalledWith(spy, arg1, arg2, ...)

    Passes if the ``spy`` was always called with the specified arguments.
    Other arguments may have been passed after the specified ones.

    ::

        var spy = this.spy();
        var arr = [1, 2, 3];
        spy("Hey", arr, 12);
        spy("Hey", arr, 13);

        assert.alwaysCalledWith(spy, "Hey");          // Passes
        assert.alwaysCalledWith(spy, "Hey", arr);     // Passes
        assert.alwaysCalledWith(spy, "Hey", arr, 12); // Fails

    **Messages**

    ::

        assert.alwaysCalledWith.message = "Expected ${0} to always be called with arguments ${1}${2}";
        refute.alwaysCalledWith.message = "Expected ${0} not to always be called with arguments${1}${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The expected arguments
    ``${2}``:
        String representation of all calls.


.. function:: calledOnceWith

    ::

        assert.calledOnceWith(spy, arg1, arg2, ...)

    Passes if the ``spy`` was called exactly once and with the specified arguments.
    Other arguments may have been passed after the specified ones.

    ::

        var spy = this.spy();
        var arr = [1, 2, 3];
        spy(12);

        assert.calledOnceWith(spy, 12);     // Passes
        assert.calledOnceWith(spy, 42);     // Fails

        spy(42, 13);
        assert.calledOnceWith(spy, 42, 13); // Fails

    **Messages**

    ::

        assert.calledOnceWith.message = "Expected ${0} to be called once with arguments ${1}${2}";
        refute.calledOnceWith.message = "Expected ${0} not to be called once with arguments ${1}${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The expected arguments
    ``${2}``:
        String representation of all calls.


.. function:: calledWithExactly

    ::

        assert.calledWithExactly(spy, arg1, arg2, ...)

    Passes if the ``spy`` was called at least once with exact the arguments specified.

    ::

        var spy = this.spy();
        var arr = [1, 2, 3];
        spy("Hey", arr, 12);
        spy("Hey", arr, 13);

        assert.calledWithExactly(spy, "Hey", arr, 12); // Passes
        assert.calledWithExactly(spy, "Hey", arr, 13); // Passes
        assert.calledWithExactly(spy, "Hey", arr);     // Fails
        assert.calledWithExactly(spy, "Hey");          // Fails

    **Messages**

    ::

        assert.calledWithExactly.message = "Expected ${0} to be called with exact arguments ${1}${2}";
        refute.calledWithExactly.message = "Expected ${0} not to be called with exact arguments${1}${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The expected arguments
    ``${2}``:
        String representation of all calls.


.. function:: alwaysCalledWithExactly

    ::

        assert.alwaysCalledWithExactly(spy, arg1, arg2, ...)

    Passes if the ``spy`` was always called with exact the arguments specified.

    ::

        var spy = this.spy();
        var arr = [1, 2, 3];
        spy("Hey", arr, 12);

        assert.alwaysCalledWithExactly(spy, "Hey", arr, 12); // Passes
        assert.alwaysCalledWithExactly(spy, "Hey", arr);     // Fails
        assert.alwaysCalledWithExactly(spy, "Hey");          // Fails

        spy("Hey", arr, 13);
        assert.alwaysCalledWithExactly(spy, "Hey", arr, 12); // Fails

    **Messages**

    ::

        assert.alwaysCalledWithExactly.message = "Expected ${0} to always be called with exact arguments ${1}${2}";
        refute.alwaysCalledWithExactly.message = "Expected ${0} not to always be called with exact arguments${1}${2}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The expected arguments
    ``${2}``:
        String representation of all calls.


.. function:: threw

    ::

        assert.threw(spy[, exception])

    Passes if the ``spy`` threw at least once the specified ``exception``.
    The ``exception`` can be a string denoting its type, or an actual object.
    If ``exception`` is not specified, the assertion passes if the ``spy`` ever threw any exception.

    ::

	var exception1 = new TypeError();
	var exception2 = new TypeError();
	var exception3 = new TypeError();
	var spy = this.spy(function(exception) {
	    throw exception;
	});
	function callAndCatchException(spy, exception) {
	    try {
	        spy(exception);
	    } catch(e) {
	    }
	}

	callAndCatchException(spy, exception1);
	callAndCatchException(spy, exception2);

	assert.threw(spy);              // Passes
	assert.threw(spy, "TypeError"); // Passes
	assert.threw(spy, exception1);  // Passes
	assert.threw(spy, exception2);  // Passes
	assert.threw(spy, exception3);  // Fails

	callAndCatchException(spy, exception3);
	assert.threw(spy, exception3); 	// Passes

    **Messages**

    ::

        assert.threw.message = "Expected ${0} to throw an exception${1}";
        refute.threw.message = "Expected ${0} not to throw an exception${1}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The expected ``exception``


.. function:: alwaysThrew

    ::

        assert.alwaysThrew(spy[, exception])

    Passes if the ``spy`` always threw the specified ``exception``.
    The ``exception`` can be a string denoting its type, or an actual object.
    If ``exception`` is not specified, the assertion passes if the ``spy`` ever threw any exception.

    ::

	var exception1 = new TypeError();
	var exception2 = new TypeError();
	var spy = this.spy(function(exception) {
	    throw exception;
	});
	function callAndCatchException(spy, exception) {
	    try {
	        spy(exception);
	    } catch(e) {
	    }
	}

	callAndCatchException(spy, exception1);

	assert.alwaysThrew(spy);              // Passes
	assert.alwaysThrew(spy, "TypeError"); // Passes
	assert.alwaysThrew(spy, exception1);  // Passes

	callAndCatchException(spy, exception2);
	assert.alwaysThrew(spy);              // Passes
	assert.alwaysThrew(spy, "TypeError"); // Passes
	assert.alwaysThrew(spy, exception1);  // Fails

    **Messages**

    ::

        assert.alwaysThrew.message = "Expected ${0} to always throw an exception${1}";
        refute.alwaysThrew.message = "Expected ${0} not to always throw an exception${1}";

    ``${0}``:
        The ``spy``
    ``${1}``:
        The expected ``exception``


.. _expectations:

Expectations
============

All of buster-assertion's assertions and refutations are also exposed as
"expectations". Expectations is just a slightly different front-end to the same
functionality, often preferred by the BDD inclined.

Expectations mirror assertions under different names. Refutations can be
expressed using ``expect(obj).not`` and then calling either of the expectations
on the resulting object.

::

    var expect = buster.assertions.expect;

    expect({ id: 42 }).toBeObject(); // Passes
    expect("Somewhere in here").toMatch("in"); // Passes
    expect(42).not.toEqual(43); // Passes


.. function:: expect.toBe

    ::

        expect(actual).toBe(expected)

    See :func:`same`


.. function:: expect.toEqual

    ::

        expect(actual).toEqual(expected)

    See :func:`equals`


.. function:: expect.toBeGreaterThan

    ::

        expect(actual).toBeGreaterThan(expected)

    See :func:`greater`


.. function:: expect.toBeLessThan

    ::

        expect(actual).toBeLessThan(expected)

    See :func:`less`


.. function:: expect.toBeDefined

    ::

        expect(actual).toBeDefined(expected)

    See :func:`defined`


.. function:: expect.toBeNull

    ::

        expect(actual).toBeNull(expected)

    See :func:`isNull`


.. function:: expect.toMatch

    ::

        expect(actual).toMatch(expected)

    See :func:`match`


.. function:: expect.toBeObject

    ::

        expect(actual).toBeObject(expected)

    See :func:`isObject`


.. function:: expect.toBeFunction

    ::

        expect(actual).toBeFunction(expected)

    See :func:`isFunction`


.. function:: expect.toBeTrue

    ::

        expect(actual).toBeTrue()

    See :func:`isTrue`


.. function:: expect.toBeFalse

    ::

        expect(actual).toBeFalse()

    See :func:`isFalse`


.. function:: expect.toBeString

    ::

        expect(actual).toBeString()

    See :func:`isString`


.. function:: expect.toBeBoolean

    ::

        expect(actual).toBeBoolean()

    See :func:`isBoolean`


.. function:: expect.toBeNumber

    ::

        expect(actual).toBeNumber()

    See :func:`isNumber`


.. function:: expect.toBeNaN

    ::

        expect(actual).toBeNaN()

    See :func:`isNaN`


.. function:: expect.toBeArray

    ::

        expect(actual).toBeArray()

    See :func:`isArray`


.. function:: expect.toBeArrayLike

    ::

        expect(actual).toBeArrayLike()

    See :func:`isArrayLike`


.. function:: expect.toThrow

    ::

        expect(actual).toThrow(expected)

    See :func:`exception`


.. function:: expect.toBeNear

    ::

        expect(actual).toBeNear(expected, delta)

    See :func:`near`


.. function:: expect.toHavePrototype

    ::

        expect(actual).toHavePrototype(prototype)

    See :func:`hasPrototype`


.. function:: expect.toContain

    ::

        expect(haystack).toContain(needle)

    See :func:`contains`


.. function:: expect.toHaveTagName

    ::

        expect(actual).toHaveTagName(expected)

    See :func:`tagName`


.. function:: expect.toHaveClassName

    ::

        expect(actual).toHaveClassName(expected)

    See :func:`className`


.. function:: expect.toHaveBeenCalled

    ::

        expect(spy).toHaveBeenCalled()

    See :func:`called`


.. function:: expect.toHaveBeenCalledOnce

    ::

        expect(spy).toHaveBeenCalledOnce(expected)

    See :func:`calledOnce`


.. function:: expect.toHaveBeenCalledTwice

    ::

        expect(spy).toHaveBeenCalledTwice(expected)

    See :func:`calledTwice`


.. function:: expect.toHaveBeenCalledThrice

    ::

        expect(spy).toHaveBeenCalledThrice(expected)

    See :func:`calledThrice`


.. function:: expect.toHaveBeenCalledWith

    ::

        expect(spy).toHaveBeenCalledWith(arg1, arg2, ...)

    See :func:`calledWith`


.. function:: expect.toHaveBeenCalledOnceWith

    ::

        expect(spy).toHaveBeenCalledOnceWith(arg1, arg2, ...)

    See :func:`calledOnceWith`


Methods
=======

.. function:: assertions.fail

    ::

        buster.assertions.fail(message)

    When an assertion fails, it calls :func:`assertions.fail` with the failure
    message as the only argument. The built-in ``fail`` function both throws an
    :class:`AssertionError` and emits it to the `failure <#event-failure>`_
    event. The error can be caught and handled by the test runner. If this
    behavior is not suitable for your testing framework of choice, you can
    override :func:`assertions.fail` to make it do the right thing.

    Example: To use **buster-assertions** with JsTestDriver, you can simply
    configure it as follows::

        buster.assertions.fail = function (message) {
            fail(message);
        };

    Where the global ``fail`` function is the one provided by JsTestDriver.

    It is possible to make the default ``assert.fail`` method only emit an
    event and not throw an error. This may be suitable in asynchronous test
    runners, where you might not be able to catch exceptions. To silence
    exceptions, see the :attr:`throwOnFailure` property.


.. function:: assertions.format

    ::

        buster.assertions.format(object)

    Values inserted into assertion messages using the ``${n}`` switches are
    formatted using :func:`assertions.format`. By default this method simply
    coerces the object to a string.

    A more expressive option is to use :ref:`buster-format`, which is a generic
    function for formatting objects nicely as ASCII. For nice ASCII formatting
    of objects (including DOM elements) do::

        buster.assertions.format = buster.format.ascii;


.. function:: assertions.add

    ::

        buster.assertions.add(name, options)

    Add a custom assertion. Using this 'macro' to add project specific
    assertions has a few advantages:

    - Assertions will be counted.

    - Failure messages will have interpolated arguments formatted by
      :func:`assertions.format`.

    - A single function generates both an assertion and a refutation.

    - If using expectations, an expectation can easily be generated as well.

    - When ```failOnNoAssertions`` <#failOnNoAssertions>`_ is set to ``true``,
      the assertion will behave correctly (may be important for asynchronous
      tests).

    - The assertion will fail if too few arguments are passed.

    Here's an example of adding a "foo" assertion, that only passes when its
    only argument is the string "foo"::

        var assert = buster.assertions.assert;
        var refute = buster.assertions.refute;
        var expect = buster.assertions.expect;

        buster.assertions.add("isFoo", {
            assert: function (actual) {
                return actual == "foo";
            },
            assertMessage: "Expected ${0} to be foo!",
            refuteMessage: "Expected not to be foo!",
            expectation: "toBeFoo"
        });

        // Now you can do:
        // Passes
        assert.isFoo("foo");

        // Fails: "[assert.isFoo] Expected { id: 42 } to be foo!"
        assert.isFoo({ id: 42 });

        // Fails: "[refute.isFoo] Expected not to be foo!"
        refute.isFoo("foo");

        // Passes
        expect("foo").toBeFoo();

        // To support custom messages, do this:
        buster.assertions.add("isFoo", {
            assert: function (actual) {
                return actual == "foo";
            },
            assertMessage: "${1}Expected ${0} to be foo!",
            refuteMessage: "${1}Expected not to be foo!",
            expectation: "toBeFoo",
            values: function (thing, message) {
                return [thing, message ? message + " " : ""];
            }
        });

        // Fails: "[assert.isFoo] Ouch: Expected { id: 42 } to be foo!"
        assert.isFoo({ id: 42 }, "Ouch");

    **Error message value interpolation**

    Arguments are available in assertion failure messages using the ``"${n}"``
    switches, where ``n`` is a number. You can also use named variables by
    setting properties on ``this`` in the assertion/refutation function::

        buster.assertions.add("isString", {
            assert: function (actual) {
                this.actualType = typeof actual;
                return this.actualType == "string";
            },
            assertMessage: "Expected ${0} (${actualType}) to be string",
            refuteMessage: "Expected not to be string",
            expectation: "toBeString"
        });

    **Arguments**

    ``name``:
        The name of the new assertion/refutation.

    ``options``:

        ``assert``:

            The verification function. Should return ``true`` when the
            assertion passes. The generated refutation will pass when the
            function returns false.

            In some cases the refutation may not be the exact opposite of the
            assertion. If that is the case you should provide
            ``options.refute`` for the custom refutation.

            The number of formal parameters the function accepts determines the
            number of required arguments to the function. If the assertion is
            called with less arguments than expected, Buster will fail it
            before your custom function is even called.

            All arguments are available for interpolation into the resulting
            error message. The first argument will be available as ``"${0}"``,
            the second as ``"${1}"`` and so on. If you want to embed other
            values than exact arguments into the string, you can set properties
            on ``this`` in the custom assertion, and refer to them as
            ``"${name}"`` in the message.

        ``refute``:

            Custom refutation function. Used over ``!assert()`` if provided.

        ``assertMessage``:

            The error message to use when the assertion fails. The message may
            refer to arguments through switches like ``"${0}"`` and so on (see
            above, under the ``assert`` argument). The message is exposed on
            the generated assertion as the property ``assert.[name].message``.

        ``refuteMessage``:

            Like ``assertFail``, but for refutations. Exposed as
            ``refute.[name].message``.

        ``values``:

            A function that maps values to be interpolated into the failure
            messages. This can be used when you need something more/else than
            the actual arguments in order.

        ``expectation``:

            The name of the assertion as an expectation, e.g. "toBeSomething".
            Optional.


Supporting utilities
====================


.. function:: buster.isNode

    ::

        buster.isNode(object)

    Returns ``true`` if the object is a DOM node. The check is made by
    attempting to call ``appendChild`` on it, passing in an element.

.. function:: buster.isElement

    ::

        buster.isElement(object)

    Returns ``true`` if the object is a DOM element. The check is made
    by calling :func:`buster.isNode` and asserting that the element's
    ``nodeType`` is 1 (i.e. element).

.. function:: assertions.isArguments

    ::

        buster.assertions.isArguments(object)

    Returns true if the argument is an ``arguments`` object. Buster checks this
    by making sure the object is array-like, but not actually an array.

    ::

        function check() {
            buster.isArguments(arguments); // true
        }

        buster.isArguments([]); // false


.. function:: assertions.keys

    ::

        buster.assertions.keys(object)

    Cross-browser implementation of
    `Object.keys <https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Object/keys>`_.
    From MDN:

    | returns an array whose elements are strings corresponding to the
    | enumerable properties found directly upon object. The ordering of the
    | properties is the same as that given by looping over the properties of
    | the object manually.


Properties
==========


.. attribute:: assertions.count

    Number increasing from 0.

    ``buster.assertions.count`` is incremented anytime an assertion is called.
    The assertion counter can be reset to any number at your convenience.


.. attribute:: throwOnFailure

    Boolean.

    When using the default :func:`assertions.fail` implementation, this
    property can be set to ``false`` to make assertion failures **not** throw
    exceptions (i.e. only emit events). This may be suitable in asynchronous
    test runners, where you might not be able to catch exceptions.


Supporting objects
==================

.. class:: AssertionError

    An exception (specifically, an `Error object
    <https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Error>`_)
    whose ``name`` property is ``"AssertionError"``.
