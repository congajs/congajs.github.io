---
layout: documentation
title: REST - CongaJS Documentation - Congajs.com
nav: documentation
---

REST
====

CongaJS provides a powerful architecture to create RESTful APIs for your application with minimal effort.

REST Controllers
----------------

To create a REST endpoint for a model, you simply need to create a controller which contains the @RestController
annotation. The following example will use CongaJS's Bass REST adapter.

<?prettify linenums=1?>
    // my-bundle/lib/controller/user.js

    /**
     * @Route("/user")
     * @RestController(adapter="conga-bass:rest/adapter", 
     *                 model="User",
     *                 documentManager="mongodb.default")
     */
    function UserController(){};

    module.exports = UserController;

Now, the following REST routes are automatically set up:

    GET    http://localhost:3000/user
    GET    http://localhost:3000/user/[id]
    POST   http://localhost:3000/user
    PUT    http://localhost:3000/user/[id]
    DELETE http://localhost:3000/user/[id]

You may also add additional controller actions by adding them to your controller's prototype just like
a normal CongaJS controller.

Querying API
------------

Out of the box, the default GET endpoint will respond to various query parameters to be able to filter, sort,
and limit the results.

### Query Parameters

#### criteria

This parameter allows you to filter your results by passing in filter conditions.

Example:

    http://localhost:3000/user?criteria={isActive:true}

#### sort

This parameter allows you to sort the results by property names.

Example:

    // sort by id ascending
    http://localhost:3000/user?sort=id

    // sort by id descending
    http://localhost:3000/user?sort=-id

    // sort by multiple fields
    http://localhost:3000/user?sort=id,-username

#### limit

This parameter allows you to limit the number of results that are returned.

    http://localhost:3000/user?limit=10

#### skip

This parameter allows you to specify how many results to skip

    http://localhost:3000/user?skip=10

    // combined with limit for paginatation
    http://localhost:3000/user?skip=10&limit=10

Enabling Paginated Responses
----------------------------

If you would like your responses for multiple results to return with pagination info, you can easily enable this
feature by passing in the "wrappedPagination" parameter to @RestController.

Example:

<?prettify linenums=1?>
    @RestController(adapter="conga-bass:rest/adapter", 
                    model="User", 
                    documentManager="mongodb.default",
                    wrappedPagination=true)

This will return an object response containing the results as an array and a "totalResults" property
which specifies the total number of results from the data source.

<?prettify?>
    {
    	"results" : [
    		{ "id" : 1, "username" : "User1", "isActive" : true },
    		{ "id" : 2, "username" : "User2", "isActive" : false }
    	],

    	"totalResults" : 100
    }

Adapters
--------

Creating a Custom Adapter
-------------------------


If you want to create a REST API for data that is not coming from Bass, you can easily implement your
own REST adapters and configure your controller to use them from the @RestController annotation.

### Create Adapter File

<?prettify linenums=1?>
    // my-bundle/lib/rest/my-adapter.js
    function MyAdapter(container, model, options){

    	this.container = container;
    	this.model = model;

    	// parse any custom options
    	if (typeof options['myOption'] !== 'undefined'){
    		this.myOption = options['myOption'];
    	}
    }

    MyAdapter.prototype = {

    	/**
    	 * The CongaJS service container
    	 * @type {Container}
    	 */
    	container: null,

    	/**
    	 * The model name
    	 * @type {String}
    	 */
    	model: null,

    	/**
    	 * My custom option defined in @RestController
    	 * @type {String}
    	 */
    	myOption: "default value",

    	/**
    	 * Find all models based on criteria, etc.
    	 * GET /endpoint
    	 */
    	findAll: function(req, res){

    	},

    	/**
    	 * Find a model by id
    	 * GET /endpoint/[id]
    	 */
    	find: function(req, res){

    	},

    	/**
    	 * Create a new model
    	 * POST /endpoint
    	 */
    	create: function(req, res){

    	},

    	/**
    	 * Update a model
    	 * PUT /endpoint/[id]
    	 */
    	update: function(req, res){

    	},

    	/**
    	 * Delete a model
    	 * DELETE /endpoint/[id]
    	 */
    	destroy: function(req, res){

    	}
    };

    module.exports = MyAdapter;

### Use your Adapter

<?prettify linenums=1?>
    @RestController(adapter="my-bundle:rest/my-adapter", 
                    model="User", 
                    myOption="Hello")

## Customizing REST Serialization




