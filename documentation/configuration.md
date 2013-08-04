---
layout: documentation
title: Configuration - CongaJS Documentation - Congajs.com
nav: documentation
---

# Configuration

CongaJS's configuration system allows you to configure every bit of your application even
depending on the current execution environment.

All configuration files for an application are located in: <span class="text-success">[my-project]/app/config</span>

## bundles.yml

This file lets you specify which bundles are loaded for the current application. CongaJS
will automatically locate the configured bundle names in <span class="text-success">[my-project]/node_modules/[bundle name]</span> or
<span class="text-success">[my-project]/bundles/[bundle name]</span>

Example:

<?prettify lang=yaml?>
	# app/config/bundles.yml
	bundles:

	    # project bundles (from bundles directory)
	    - frontend-bundle

	    # core conga bundles (from node_modules directory)
	    - conga-assets
	    - conga-bass
	    - conga-bower
	    - conga-browserify
	    - conga-client-template
	    - conga-cluster
	    - conga-jade
	    - conga-less
	    - conga-passport
	    - conga-pubsub
	    - conga-socketio

## config.yml

This is the main configuration file for your application.

## config_\[environment\].yml

These environment specific config files allow you to override settings in the default config.yml specific
to the associated environment. This allows you to define things such as logging levels specific to each
environment.

## parameters.ini

This file contains any specific configuration settings that are specific to the current running environment such
as database connections, passwords, etc. By default this file is added to the <span class="text-success">.gitignore</span> to keep sensitive information
out of source control. However, you can add default settings to the parameters.ini.dist file so that other developers
can copy that file to <span class="text-success">parameters.ini</span> for their local environment.
