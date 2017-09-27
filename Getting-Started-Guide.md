## Setup

### Create a basic go project

    export GOPATH=$HOME/dosa-sample
    mkdir -p $GOPATH/src/dosa-sample
    cd $GOPATH/src/dosa-sample

### Install glide

If you don't already have it, see https://github.com/Masterminds/glide/blob/master/README.md#install for specific instructions.

### Make sure `glide` and `$GOPATH/bin` are in your `$PATH`:

Starting in the base directory of your sources:

    export PATH="$PATH:$GOPATH/bin"

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

## Initialize client

Create main.go as follows:

package main

    import (
	"context"
	"github.com/uber-go/dosa"
	_ "github.com/uber-go/dosa/connectors/cassandra"
    )

    func main() {
        registry, err := dosa.NewRegistrar("dosakeyspace", "appname", &MyFirstDosaObject{})
        if err != nil {
                panic(err.Error())
        }
        connector, err := dosa.GetConnector("cassandra", nil)
        if err != nil {
                panic(err.Error())
        }
        client := dosa.NewClient(registry, connector)
        if err := client.Initialize(context.TODO()); err != nil {
                panic(err.Error())
        }
        if err := client.Upsert(context.TODO(), dosa.All(), &MyFirstDosaObject{K1: dosa.NewUUID(), K2: "testdata", K3: 1, C1: time.Now()}); err != nil {
                panic(err.Error())
        }
    }

### Install the DOSA library

    glide init

### Install the DOSA CLI

    go get -u github.com/uber-go/dosa/cmd/dosa

The `dosa` binary should now be installed in `$GOPATH/bin`.

    ls -l $GOPATH/bin/dosa

## Get all the dependencies

    glide install
    go build

### Dump your schema definitions

Do this to check for errors in your entities, use the CLI's `schema dump` command, `prefix` is required:

    dosa schema dump

This command takes an optional list of directories to look for entities. If none are provided, it will default to the current directory. Since the example assumes entities are defined in `./entities.go`, no arguments are needed.

### Install your schema into a cassandra instance

    echo "create keyspace dosakeyspace with replication = { 'class': 'SimpleStrategy', 'replication_factor': 1 };" | cqlsh
    dosa schema dump | cqlsh

### Run the app

Now just build and run the app like this:

    go build
    ./dosa-sample

If it worked, you'll find a row in the Cassandra database:

    echo 'select * from dosakeyspace.myfirstdosaobject;' | cqlsh
     k1                                   | k2       | k3 | c1
    --------------------------------------+----------+----+---------------------------------
     b68281f9-4096-4e23-9359-a3b28ebc026e | testdata |  1 | 2017-09-27 10:44:12.799000+0000
