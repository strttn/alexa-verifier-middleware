# alexa-verifier-middleware

[![NPM](https://nodei.co/npm/alexa-verifier-middleware.png)](https://www.npmjs.com/package/alexa-verifier-middleware/)

[![dependencies Status](https://david-dm.org/tejashah88/alexa-verifier-middleware/status.svg)](https://david-dm.org/tejashah88/alexa-verifier-middleware)
![NPM Version](https://img.shields.io/npm/v/alexa-verifier-middleware.svg)

Verify HTTP requests sent to an Alexa skill are sent from Amazon.

This is an [express](https://www.npmjs.com/package/express) middleware that wraps logic in the [alexa-verifier](https://www.npmjs.com/package/alexa-verifier) module.


### Usage

```javascript
var express = require('express');
var avm = require('alexa-verifier-middleware');
var app = express();

app.use(avm()); // install the verifier middleware

// other body parsers, etc. follow...
```


### Usage with other body parser middlewares
This middleware needs to read the entire request body, and express doesn't expose that on the request object.
If you are using any other body parsing middleware, you must load this one first.

Example:

```javascript
var express = require('express');
var bodyParser = require('body-parser');
var avm = require('alexa-verifier-middleware');
var app = express();

// note that the 'avm' middleware is loaded first
app.use(avm());

// now you can load other body parser related middlewares here
app.use(bodyParser.urlencoded({extended: true}));
app.use(bodyParser.json());
```

If there is another middleware that says it needs to load first, and you are not sure which placement is better, let me know by opening an issue and I'll do some extensive testing before reporting which middleware to load first.


### Extra options

* `strictHeaderCheck` - Allows for strict handling of incoming requests to make sure they only come from Amazon's servers. By default this check will **not** be enforced.
    
    Example:

    ```javascript
    var express = require('express');
    var avm = require('alexa-verifier-middleware');
    var app = express();
    
    var alexaRouter = express.Router();
    alexaRouter.use(avm({ strictHeaderCheck: true }));
    
    // Routes that handle alexa traffic are now attached here.
    // Since this is attached to a router mounted at /alexa,
    // this endpoint will be accessible at /alexa/weather_info
    alexaRouter.get('/weather_info', function(req, res) { ... });
    
    app.use('/alexa', alexaRouter);
    
    app.listen(3000)
    ```

### Mentions
* [mreinstein](https://github.com/mreinstein) for his [alexa-verifier](https://github.com/mreinstein/alexa-verifier) module, which allows you to verify any Amazon requests from any web service

### License
Copyright (c) 2016-2017 Tejas Shah

MIT License, see [LICENSE](https://tejashah88.mit-license.org/2016-2017) for details.