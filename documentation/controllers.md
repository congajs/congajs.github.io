---
layout: documentation
title: Controllers - CongaJS Documentation - Congajs.com
nav: documentation
---

# Controllers

CongaJS controllers are files which contain actions that are associated with routes or websocket actions.

## Generate a Controller

<?prettify lang=bash"?>
    $ conga generate:controller my-bundle hello




<?prettify linenums=1?>
    // my-bundle/lib/controller/default.js
	function DefaultController(){};

	DefaultController.prototype = {

	    /**
	     * @Route("/hello")
	     */
	    hello: function(req, res){
		  res.return({ message : "Woohoo!!! This is my hello action!!!" });
	    }
	}

	module.exports = DefaultController;

By default, this will return a JSON response when the url is requested at http://localhost:3000/hello

To associate this action with a template view, simply add the @Template annotation:

<?prettify linenums=1?>
    /**
     * @Route("/hello")
     * @Template
     */
    hello: function(req, res){
    	res.return({ message : "Now I'm using a template!!!" });
    }

This will automatically let CongaJS know that it should look for a corresponding view at:

<p class="text-success">my-bundle/lib/view/default/hello.[view-engine]</p>

Request and Response
--------------------

The request and response objects supplied to a controller action are simply ExpressJS request
and response objects with some additional functionality.

Usually, you will use the Response.return() method to return a response no matter if your action
is configured to return a template, JSON, or a websocket response.

The return() method will pass the response value through any additional post filters and return
the correct format that is configured for the controller action.

Accessing the Service Container
-------------------------------

Every controller is automatically provided the [CongaJS service container instance](/documentation/service-container.html). To access services
from the container, simply call this.container

Example:

<?prettify linenums=1?>
    /**
     * @Route("/container-demo")
     */
    containerDemo: function(req, res){
        this.container.get('logger').info('Using the logger from the container!!!');
        res.return({});
    }


## @Route


The @Route annotation is used to map a controller action to a specified url and optionally map
the HTTP methods that the action should respond to.

Full @Route example:

<?prettify linenums=1?>
    /**
     * @Route("/hello", name="default.hello", methods=["GET","POST"])
     */
    
### Parameters:

[url]
  : The url to map to.

name
: A unique name for the route. This name can be referenced in other parts of the server application or the client-side code. This allows you to be able to change action urls within the annotation without affecting any code that is referencing the name.

methods
  : An array of HTTP methods that the action responds to. Possible values are GET, POST, PUT, DELETE

  : The @Route annotation can also be applied to the controller constructor function to specify a prefix for all actions within the controller:

<?prettify linenums=1?>
    /**
     * @Route("/about")
     */
	function AboutController(){};

	AboutController.prototype = {

		/**
		 * maps to: /about/company
		 * @Route("/company")
		 */
		company: function(req, res){
			//
		},

		/**
		 * maps to: /about/jobs
		 * @Route("/jobs")
		 */
		jobs: function(req, res){
			//
		},
	}

	module.exports = AboutController;


## @Template

The @Template annotation is used to map an action to a view template. By default CongaJS will look for the corresponding view in the following location:

[bundle]/lib/view/[controller-file-name]/[action-name].[view-engine]

This can be overridden by providing the @Template annotation with a namespaced path of a template file:

<?prettify linenums=1?>
    /**
     * Use my-bundle/lib/view/default/other.[view-engine]
     *
     * @Template("my-bundle:default/other")
     */
    
## @PreFilter and @PostFilter

The @PreFilter and @PostFilter annotations allow you to define middleware that should be run pre and post execution
of a controller action (or all actions in a controller).

Example:

<?prettify linenums=1?>
    /**
     * @PreFilter("my.pre-controller.filter")
     * @PostFilter("my.post-controller.filter")
     */
    var MyController = function(){};

    MyController.prototype = {

    	/**
    	 * @PreFilter("my.pre-action-filter")
    	 * @PostFilter("my.post-action-filter")
    	 */
    	hello: function(req, res){
            //
    	}
    }

### Parameters:

#### [filter namespace]

This is the file to use as the filter


### params

You may optionally define a hash of parameters that should be passed to the filter function on execution.

Example:

<?prettify linenums=1?>
    /**
     * @PreFilter("my.pre-action-filter", params={someValue:"foo"})
     */
    
## @SSL

The @SSL annotation specifies that a controller or an individual action in a controller should be forced
to use an HTTPS connection.

When the @SSL annotation is placed on a controller constructor, all actions within the controller
will be forced to use HTTPS.

Controller Example:

<?prettify linenums=1?>
    /**
     * @SSL
     */
    var MyController = function(){};

    MyController.prototype = {

        /**
         * 
         */
        hello: function(req, res){
            //
        }
    }

Action Example:

<?prettify linenums=1?>
    /**
     * 
     */
    var MyController = function(){};

    MyController.prototype = {

        /**
         * @SSL
         */
        hello: function(req, res){
            //
        }
    }



## @RestController

The @RestController annotation specifies that a controller should automatically provide RESTful actions for the
specified model.

Example:

<?prettify linenums=1?>
    /**
     * @Route("/user")
     * @RestController(adapter="bass-bundle:rest/controller", 
     *                 manager="mongodb.default",
     *                 model="my-bundle:model/user")
     */
    var UserController = function(){};

    module.exports = UserController;

    


