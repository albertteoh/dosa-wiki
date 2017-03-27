## DOSA v2 Getting Started (wip)

## Setup

# Install DOSA Client library

    ./go-build/glide get github.com/uber-go/dosa

# Install the DOSA CLI (this is a hack for the time being, ideally the command above would install `dosa` into $GOPATH/bin)

    cd vendor/github.com/uber-go/dosa
    glide install
    make cli

The `dosa` binary should now be installed in `$GOPATH/bin`. If this isn't already in your $PATH, you may want to add it:

    export PATH=$PATH:$GOPATH/bin

# Start Cerberus to proxy to `dosa-gateway`

    cerberus -t dosa-gateway --dc sjc1

# Create your DOSA developement scope

    # may have to override host and port
    dosa scope create eculver


# TODO: Define entities
# TODO: Dump/check/upsert schema
# TODO: Initialize client
# TODO: Create/query data
