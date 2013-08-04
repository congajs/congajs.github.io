---
layout: documentation
title: Command Line Interface - CongaJS Documentation - Congajs.com
nav: documentation
---

Command Line Interface
======================

CongaJS provides an API for you to easily create commands that hook into the global "conga" command.
This allows you to easily create command line utilities that leverage your configured CongaJS resources.

Creating a new Command
----------------------

CongaJS will automatically look for a "command" directory in each registered bundle, so simply create
this directory and a new command javascript file.

Example:

    my-bundle/lib/command/say-something-cool.js

Now implement the code for your command:

<?prettify linenums=1?>
	// example command
	module.exports = {

		/**
		 * Set up configuration for this command
		 * 
		 * @var {Object}
		 */
		config: {
			command: "say:something:cool <message>",
			description: "Just an example command",
			options: {
				'num' : ['-n, --num [value]', 'How many times to say it']
			},
			arguments: ['message']
		},

		/**
		 * Run the command
		 * 
		 * @return {void}
		 */
		run: function(container, args, options, cb){

			// get the "num" value from options
			var num = (typeof options['num'] !== undefined) ? options['num'] : 1;

			for(var i=0; i<num; i++){
				// print the message
				console.log(args['message']);
			}

			cb();
		}
	};

Run the Command
---------------

<?prettify lang=bash?>
    conga say:something:cool "Conga Rocks!!!" --num 10
