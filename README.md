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
         handler: server.methods.handler.getPage({name:'main']}),
        }
    ])
   
});
```

- `options` Object with the following keys
  - `name` [optional] name of object that are attached to server.methods holding library function/s
  - `templates` Array List with names of parts files to include
  - `paths` Object with the following keys
    - `director` String Path relative view folder to `director.html` file
    - `parts` String Path relative view folder to `parts` directory
    - `pages` String Path relative view folder to `pages` directory
    - `views` String Path to view folder

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
- `options` Object with the following keys
  - `name` [optional] name of object that are attached to server.methods holding library function/s
  - `templates` Array List with names of parts files to include
  - `paths` Object with the following keys
    - `director` String Path relative view folder to `director.html` file
    - `parts` String Path relative view folder to `parts` directory
    - `pages` String Path relative view folder to `pages` directory
    - `views` String Path to view folder

**Kind**: static method of <code>[handler](#server.methods.module_handler)</code>  
<a name="server.methods.module_handler..getOrchestraView"></a>

### handler~getOrchestraView() ⇒ <code>Promise</code>
Create a view by composing files in the  `templates` directory
 through the `director.html`

- `options` Object with the following keys
  - `callbacks` Array with functions `function(request, params, done)
  - `exclude` String [optional] Name of part files to exclude (without `.html` ending)
  - `include` String [optional] Name of part files to include (without `.html` ending)
  - `params` Object `{key:values}` made available to templates for `handlebars.compile`
  - `substitute` object with contains `key:value` pairs where key is a
  default template name (excluding .html)  and value is relative or full
  path to a substitute template.

**Kind**: inner method of <code>[handler](#server.methods.module_handler)</code>  
## Test
`npm run-script test`

## Contributing
In lieu of a formal styleguide, take care to maintain the 
existing coding style. Add unit tests for any new or changed 
functionality. Lint and test your code.

## Release History