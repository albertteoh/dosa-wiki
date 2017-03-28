## Setup

### Create a basic go project

### Make sure `glide` and `$GOPATH/bin` are in your `$PATH`:

Starting in the base directory of your sources:

    export PATH="$PATH:$GOPATH/bin"

### Install the DOSA library

    glide get github.com/uber-go/dosa

### Install the DOSA CLI

    go get -u github.com/uber-go/dosa/cmd/dosa

The `dosa` binary should now be installed in `$GOPATH/bin`.

    ls -l $GOPATH/bin/dosa

### TODO: Start standalone gateway

    make start-gateway

### Create your DOSA development scope

    dosa scope create <scope_name>

## Build your data model

Modeling your data is beyond the scope of this quick start guide. Please reach out to us for help!

### Create at least one entity

In your code, create an object that embeds `(github.com/uber-go/dosa).Entity`, we'll call it `entities.go`:

    package main

    import (
        "time"

        "github.com/uber-go/dosa"
    )

    type MyFirstDosaObject struct {
        dosa.Entity `dosa:"primaryKey=((K1, K2), K3)"`
        K1 dosa.UUID
        K2 string
        K3 int64
        C1 time.Time
    }

This example describes an object with a partitioning key of `(K1, K2)` and
a clustering key of `K3`. There is a single non-key field, `C1`.

### Dump your schema definitions

Do this to check for errors in your entities, use the CLI's `schema dump` command:

    dosa schema dump

This command takes an optional list of directories to look for entities. If none are provided, it will default to the current directory. Since the example assumes entities are defined in `./entities.go`, no arguments are needed.

### Install your schema with the `schema upsert` command:

    dosa schema upsert

This will translate your DOSA entities into the associated database schema. If the underlying datastore is Cassandra, this will translate to CQL. For a preview of the schema that will be installed, use the `schema dump` command can again be used. In this case, to preview the CQL pass the `format` option:

    dosa schema dump --format cql

    create table "myfirstdosaobject" ("k1" uuid, "k2" text, "k3" bigint, "c1" timestamp, primary key ((k1, k2), k3 ASC));

## TODO: Initialize client
## TODO: Create/query data
