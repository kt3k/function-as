# function-as

## The idea

```js

var method = function () {

    //...
    //...

}
.as(decorator0)
.as(decorator1);

```


## The implementation

```js
Function.protoytpe.as = function (decorator) {

    return decorator(this);

};
```


## Examples


### Chainable

```js
var Chainable = function (func) {

    return function () {

        func.apply(this, arguments);

        return this;

    };

};


var method = function () {
    //...
    //...
    //...
}
.as(Chainable);

// Then this method returns the object itself (= chainable)

```


### Promised Error Handling

```js

// suppose `func` is an api request handler in express

var ErrorHandler = function (func) {

    return function (req, res) {

        var that = this;

        return Promise.resolve().then(function () {

            return func.call(that, req, res);

        }).catch(function (err) {

            res.status(500).json({errors: [msg: 'uncaught error']});

            someComplexLogging(err);

        });

    });

};



var apiHandler = function () {

    //...
    //...
    //...

}
.as(ErrorHandler);

// Then this method automatically handles uncaught errors and send a response to the client

```
