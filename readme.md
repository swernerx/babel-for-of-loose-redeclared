# Duplicate variable declaration bug

- `npm install`
- `node_modules/.bin/babel entry.js`

Output looks like:

```js
"use strict";

async function executeTasks(tasks) {
  for (var _iterator = tasks, _isArray = Array.isArray(_iterator), _i = 0, _iterator = _isArray ? _iterator : _iterator[Symbol.iterator]();;) {
    var _ref;

    if (_isArray) {
      if (_i >= _iterator.length) break;
      _ref = _iterator[_i++];
    } else {
      _i = _iterator.next();
      if (_i.done) break;
      _ref = _i.value;
    }

    var taskName = _ref;

    try {
      await executeCommands();
    } catch (error) {
      console.error("Error");
    }
  }
}
```

In line 4 we can see that `_iterator` is being redeclared. This is kind of problematic for other plugins e.g. `fast-async` which produces a warning in this case.

Most trivial solution would be to use a new variable for the first assignment:

```js
"use strict";

async function executeTasks(tasks) {
  for (var _iteratorInput = tasks, _isArray = Array.isArray(_iteratorInput), _i = 0, _iterator = _isArray ? _iteratorInput : _iteratorInput[Symbol.iterator]();;) {
    var _ref;

    if (_isArray) {
      if (_i >= _iterator.length) break;
      _ref = _iterator[_i++];
    } else {
      _i = _iterator.next();
      if (_i.done) break;
      _ref = _i.value;
    }

    var taskName = _ref;

    try {
      await executeCommands();
    } catch (error) {
      console.error("Error");
    }
  }
}
```
