[![Build Status](https://travis-ci.org/mickelindahl/hapi_orchestra_view.svg?branch=master)](https://travis-ci.org/mickelindahl/hapi_orchestra_view)
[![Coverage Status](https://coveralls.io/repos/github/mickelindahl/hapi_orchestra_view/badge.svg?branch=master)](https://coveralls.io/github/mickelindahl/hapi_orchestra_view?branch=master)

# Hapi orchestra view
 
Library for composing page views through a director that orchestrates
templates into a view. A director.html file is used to compose
the input from a number of templates where templates
except `raw.html` are compiled using handlebars. Parameters in
`options.params` are available to all templates.
 

## Installation

Copy files in `lib/views` to preferred folder in project
 
Open the `director.html` and edit it accordingly to your preference. Each 
triple handlebar import corresponds to the compiled output (except for
`raw.html`) of files in `lib/view/templates`.

Run `npm install --save git+https://x-oauth-basic@github.com/mickelindahl/hapi_orchestra_view.git`
  

## Usage
```js
'use strict'

const Hapi = require( 'hapi' );

const server = new Hapi.Server( { port: 3000 } );

server.register( {
    register: require( 'hapi-page-view' ),
    options: { 
        views: ['head', 'nav', 'footer', 'scripts', 'raw']
    }
}).then(()=>{

    server.route([
        {
         method: 'GET',
         path: '/'
         handler: server.methods.handler.getOrchestraView({name:'main']}),
        }
    ])
   
});
```

## Methods

The `handler` is attached to hapijs `server.methods`

<a name="server.methods.module_handler"></a>

## handler

* [handler](#server.methods.module_handler)
    * _static_
        * [.register()](#server.methods.module_handler.register)
    * _inner_
        * [~getOrchestraView()](#server.methods.module_handler..getOrchestraView) ⇒ <code>Promise</code>

<a name="server.methods.module_handler.register"></a>

### handler.register()
Hapi plugin options

- `options` Object with the following keys
  - `name` [optional] name of object that are attached to server.methods holding library function/s
  - `templates` {array} List with available templates. Multiple templates having the same `name` will
  be merged
    - `[].id` {string} Template identifier
    - `[].param` {string} Director parameter name where template should be inserted
    - `[].path` {string} Path to template relative view directory
    - `[].compile` {boolean} Indicates weather template should be compiled or not
  - `directors` {array} List with available directors
      - `[].id` {string} Director identifier
      - `[].path` {string} Path to director
  - `views` {string} Path to view folder

**Kind**: static method of <code>[handler](#server.methods.module_handler)</code>  
<a name="server.methods.module_handler..getOrchestraView"></a>

### handler~getOrchestraView() ⇒ <code>Promise</code>
Create a view by composing files in the  `templates` directory
 through the `director.html`

- `options` {object} with the following keys
  - `callbacks` {array} Array with callback
    - `[](request, params, done)` {callback}
      - `request` {object} hapijs request object
      - `params` {object} object holding params used at template compilation
      - `done` {promise.resolve} Function called at completion
  - `include` {string} Template ids to include
  - `params` Object `{key:values}` made available to templates for `handlebars.compile`

**Kind**: inner method of <code>[handler](#server.methods.module_handler)</code>  
## Test
`npm run-script test`

## Contributing
In lieu of a formal styleguide, take care to maintain the 
existing coding style. Add unit tests for any new or changed 
functionality. Lint and test your code.

## Release History