## Setup

### Create a basic go project

### Make sure glide and $GOPATH/bin are on your PATH

Starting in the base directory of your sources:

    export PATH="$PWD/go-build:$GOPATH/bin:$PATH"

### Install DOSA Client library

    glide get github.com/uber-go/dosa

### Install the DOSA CLI

This is a hack for the time being, ideally the command above would install `dosa` into $GOPATH/bin

    cd vendor/github.com/uber-go/dosa
    glide install
    make cli

The `dosa` binary should now be installed in `$GOPATH/bin`.

    ls -l $GOPATH/bin/dosa

### Start Cerberus to proxy to `dosa-gateway`

    cerberus -t dosa-gateway --dc sjc1

### Create your DOSA development scope

    dosa scope create your_name_here

## Build your data model

Modeling your data is beyond the scope of this quick start guide. Please reach out to us for help!

### Create at least one entity

In your code, create an object that extends a dosa.Entity, like this:

    import "github.com/uber-go/dosa"
    type MyFirstDosaObject struct {
        dosa.Entity `dosa:"primaryKey=((K1, K2), K3)"`
        K1 dosa.UUID
        K2 string
        K3 int64
        C1 time.Time
    }

This example described a table with a partition key of (K1, K2) and
a clustering key of K3. There is a single non-key field, C1.

### Dump your schema definitions

Do this to check for errors in your entities

    dosa schema dump directory_where_your_entity_sources_are

If you leave off directory_where_your_entity_sources_are, it will scan the current directory only

### Upload your schema to the server

    dosa schema upsert directory_where_your_entity_sources_are

## TODO: Initialize client
## TODO: Create/query data
