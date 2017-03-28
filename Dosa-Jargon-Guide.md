Dosa has some jargon, which is defined here:

## scope

Scopes are used to separate developer datasets. In future versions of dosa, layering can be supported for different scopes. There is one predefined scope named 'production'.

In general, you want to test your schema using a scope that matches your name. Scopes are generally NOT stored in source code. They are usually set in configuration files.

Scopes can be created or destroyed with the dosa command line tool. The production scope cannot be destroyed.

## namePrefix

A namePrefix (sometimes called a prefix) is a constant in your application. It can be set to your service name, for example.