# Mixinable

<a href="https://travis-ci.org/dmbch/mixinable">
  <img src="https://travis-ci.org/dmbch/mixinable.svg?branch=master">
</a>

`mixinable` is a small utility library allowing you to use [mixins](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#mixinpatternjavascript) in your code. Apart from enabling you to add and override new methods to your prototypes, it helps you apply different strategies to those additional methods.

`mixinable` keeps your protoype chain intact (`instanceof` works as expected), allows you to provide custom `constructor` functions and supports asynchronous methods returning `Promises`.

### Installation

Using [NPM](https://www.npmjs.com/get-npm):

```bash
npm install -S mixinable
```

Using [Yarn](https://yarnpkg.com/en/):

```bash
yarn add mixinable
```

### API

#### ```mixin([...definition])```

The main export of `mixinable` is a variadic function accepting any number of mixin definitions. It returns a constructor/factory function that creates instances containing the mixin methods you defined.

T.B.D. - to be documented

#### ```mixin.replace([...implementation])```

T.B.D. - to be documented

#### ```mixin.parallel([...implementation])```

T.B.D. - to be documented

#### ```mixin.pipe([...implementation])```

T.B.D. - to be documented

#### ```mixin.sequence([...implementation])```

### Examples

##### Basic Example

```javascript
import mixin from 'mixinable';

// const Foo = mixin({
const createFoo = mixin({
  bar() {
    // ...
  }
});

// const foo = new Foo();
const foo = createFoo();

// console.log(foo instanceof Foo);
console.log(foo instanceof createFoo);
```

##### Multiple Mixins Example

```javascript
import mixin from 'mixinable';

// multiple mixin arguments are being merged
const createFoo = mixin(
  {
    bar() {
      // ...
    }
  },
  {
    // you can pass multiple implementations at once
    baz: [
      function () { /* ... */ },
      function () { /* ... */ }
    ]
  }
);

// constructors/factories created with mixin can be extended
const createQux = createFoo.mixin({
  bar() {
    // ...
  }
});
```

##### `parallel` Example

```javascript
import mixin, { parallel } from 'mixinable';

// const Foo = mixin({
const createFoo = mixin({
  bar: parallel(
    function () {
      return Promise.resolve(1);
    },
    function () {
      return Promise.resolve(2);
    }
  )
});

const foo = createFoo();

foo.bar().then(res => console.log(res));
// [1, 2]
```

##### `pipe` Example

```javascript
import mixin, { pipe } from 'mixinable';

// const Foo = mixin({
const createFoo = mixin({
  bar: pipe(
    function (val, inc) {
      return Promise.resolve(val + inc);
    },
    function (val, inc) {
      return (val + inc);
    }
  )
});

const foo = createFoo();

foo.bar(0, 1).then(res => console.log(res));
// 2
```

##### `sequence` Example

```javascript
import mixin, { sequence } from 'mixinable';

// const Foo = mixin({
const createFoo = mixin({
  bar: sequence(
    function (options) {
      this.baz = options.baz;
    },
    function (options) {
      this.qux = this.bar * 42;
    }
  )
});

const foo = createFoo();

foo.bar({ baz: 23 });
console.log(foo.qux);
// '966'
```

### Contributing

If you want to contribute to this project, create a fork of its repository using the GitHub UI. Check out your new fork to your computer:

```bash
mkdir mixinable && cd $_
git clone git@github.com:user/mixinable.git
```

Afterwards, you can `yarn install` the required dev dependencies and start hacking away. When you are finished, please do go ahead and create a pull request.

`mixinable` is entirely written in ECMAScript 5 and adheres to semistandard code style. Please make sure your contribution does, too.
