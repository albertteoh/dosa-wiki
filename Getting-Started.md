# DOSA v2 Getting Started (wip)

## Setup

### Install DOSA Client library

    ./go-build/glide get github.com/uber-go/dosa

### Install the DOSA CLI (this is a hack for the time being, ideally the command above would install `dosa` into $GOPATH/bin)

    cd vendor/github.com/uber-go/dosa
    glide install
    make cli

The `dosa` binary should now be installed in `$GOPATH/bin`. If this isn't already in your $PATH, you may want to add it:

    export PATH=$GOPATH/bin:$PATH

### Start Cerberus to proxy to `dosa-gateway`

    cerberus -t dosa-gateway --dc sjc1

### Create your DOSA development scope

    dosa scope create eculver

## Build your data model

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

## Dump your schema definitions

    # do this to check for errors in your entities
    dosa schema dump _sourcedir_
    # if you leave off _sourcedir_, it will scan the current directory only

## Upload your schema to the server

    dosa schema upsert _sourcedir_

## TODO: Initialize client
## TODO: Create/query data
