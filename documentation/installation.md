---
layout: documentation
title: Installation - CongaJS Documentation - Congajs.com
nav: documentation
---

# Installation

The following page will show you how to get CongaJs up and running quickly.

## Install CongaJs

To begin using CongaJs, install the global "conga" command via npm:

<?prettify lang=bash?>
    $ npm install -g conga

## Create a project

Create a project in your current work directory by issuing the following command:

<?prettify lang=bash?>
    $ conga create:project my-project

This will create a directory name "my-project" in your current directory and automatically
install all dependencies via npm.

## Start up the demo application

Change into the directory and start up the conga server

<?prettify lang=bash?>
    $ cd my-project
    $ conga play

## View demo application

Open up your browser to http://localhost:3000