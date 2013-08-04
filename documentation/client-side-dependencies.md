---
layout: documentation
title: Managing Client-side Dependencies - CongaJS Documentation - Congajs.com
nav: documentation
---

## Managing client side dependencies

Conga.js allows you to easily manage your client side javascript dependecies through [Bower](http://bower.io) by using the included [conga-bower bundle](https://github.com/congajs/conga-bower).

To add a new dependency simply add it to the bower configuration in config.yml:

<?prettify lang=yaml linenums=1?>
    # app/config/config.yml

    bower:

        directory: js/lib

        dependencies:

            jquery: 1.9.0

Now when the server is restarted, jquery will be installed through Bower into the app/public/js/lib directory.

