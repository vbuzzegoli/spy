![spy](spy.jpg)

Simple Logger as a Redux Middleware, by [Victor Buzzegoli](https://twitter.com/victorbuzzegoli)

Lightweight, _MMMM_ compliant (check out : [Modern Modular Middleware Model](https://github.com/vbuzzegoli/4m))

## Installation

To install **Spy** in your project, navigate to your project folder in your terminal and run :

    npm i --save redux-spy-logger

## Setup

To start using **Spy**, you will first need to apply the middleware to your store, just like any redux middleware :

```javascript
    ...
    import spy from "redux-spy-logger";
    ...
    export default createStore(rootReducer,applyMiddleware([spy]));
```

## Usage

> Using ES6+ syntax

```javascript
    import * as actions from "../constants/action-types";

    export const clearArray = () => {
        type: actions.CLEAR_ARRAY,
        payload: [],
        spy: {
            log: true
        }
    }
```

#### Behaviour

-   Use `onLog` to override the default behaviour

> Note that these functions are called **reactions**, accordingly to the [Modern Modular Middleware Model](https://github.com/vbuzzegoli/4m). Therefore they contain a `next` argument that can be use to release an action to the reducer (or next middleware). They can be used like :

In `/reactions` :

```javascript
export const customReaction = (action, next, dispatch) => {
	console.log("Logged Action :", action);
	next(action);
};
```

In `/actions` :

```javascript
    import * as actions from "../constants/action-types";
    import { customReaction } from "../reactions/customReaction";

    export const clearArray = () => {
        type: actions.CLEAR_ARRAY,
        payload: [],
        spy: {
            log: true,
            onLog: customReaction
        }
    }
```

> If you were to use a non 4M compliant middleware such as _redux-thunk_, which is **deprecated by the [4M documentation](https://github.com/vbuzzegoli/4m)**, please note that, by default, using/dispatching the action returned by `onLog` will not trigger _Spy_ again even though the arguments are still contained in the action's parameters. To force triggering _Spy_ again, use : `_skip: false` or remove `_skip` in the `spy` node.

-   Use `log: true` to log every action going through _Spy_ in the console

Here is a overview of every options possible:

```javascript
    spy: {
        log: true,
        onLog: (action, next, dispatch) => {
            //...
        }
    }
```

## Version

1.1.1

## License

**The MIT License**

Copyright 2018 - Victor Buzzegoli

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Contact

[@victorbuzzegoli](https://twitter.com/victorbuzzegoli) on Twitter
