DOSA uses some terminology that is specific to the project. Those terms are defined below.

## Scope

Scopes are used to separate developer datasets. In future versions of dosa, layering can be supported for different scopes. There is one predefined scope named 'production'.

In general, you want to test your schema using a scope that matches your name. Scopes are generally NOT stored in source code. They are usually set in configuration files.

Scopes can be created or destroyed with the dosa command line tool. The production scope cannot be destroyed.

## Name Prefix (aka prefix)

A "name prefix" uniquely identifies DOSA entity types. It serves as a namespace that is _prefixed_ to all DOSA entity type names in your project. It should be treated as a constant. Naming things is hard; if we have to recommend a starting point, consider a construction like "organization.service". Or, more concretely, if there is a team in your organization called "dataviz" working on a service called "charting", your prefix might be "dataviz.charting". 

## Entity

An "entity" is any object in your code that can be persisted. It loosely maps to a "table" in traditional database terminology.

## Connector

TODO: Describe what this is