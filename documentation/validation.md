---
layout: documentation
title: Validation - CongaJS Documentation - Congajs.com
nav: documentation
---

# Validation

Conga makes it easy to provide validation of your model objects and is automatically handled in the REST controllers.

First we need to configure the application to look for validation annotations on our models:

<?prettify lang=yaml linenums=1?>
    # app/config/config.yml

    validation:
        paths:
            - my-bundle:model

Now we can add the @Assert:* validation annotations to a model:

<?prettify linenums=1?>
    // src/my-bundle/lib/model/user.js

    /**
     * @Bass:Document(name="User", collection="users")
     */
    function User(){};

    User.prototype = {

        /**
         * @Bass:Id
         * @Bass:Field(type="ObjectId", name="_id")
         */
        id: null,

        /**
         * @Bass:Field(type="String")
         * @Assert:NotNull
         * @Assert:Length(min=2, max=50)
         */
        name: null,

        /**
         * @Bass:Field(type="String")
         * @Assert:NotNull
         * @Assert:Length(min=2, max=50)
         */
        email: null

    };

    module.exports = User;

Finally, test out a POST request with invalid data:

<?prettify lang=bash?>
    $ curl http://localhost:3000/user -X POST -H "Content-Type: application/json" -d '{"name":"a"}'

<?prettify?>
    {
      "success": false,
      "errors": [
        {
          "message": "This value must be between 2 and 50 characters long",
          "property": "name"
        },
        {
          "message": "This value should not be null",
          "property": "email"
        }
      ]
    }

