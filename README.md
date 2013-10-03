TileMill is a modern map design studio powered by Node.js and Mapnik.

Installation instructions, development docs and other information are available
on the [TileMill website](http://mapbox.com/tilemill).

[![Dependencies](https://david-dm.org/mapbox/tilemill.png)](https://david-dm.org/mapbox/tilemill)

# Depends

- Mapnik v2.3.0
- Node.js v0.10.x or v0.8.x
- Protobuf: libprotobuf-lite and protoc

For details follow:

- [Install packages](http://mapbox.com/tilemill/docs/install/)
- [Build from source](http://mapbox.com/tilemill/docs/source/)

# Running tests

Install mocha and run the tests

    npm install mocha
    npm test


Note: the tests require a running postgres server and a postgis enabled
database called `template_postgis`.

If you do not have a `template_postgis` create one like:

    POSTGIS_VERSION="1.5" # you may need to change this
    POSTGIS_PATH=`pg_config --sharedir`/contrib/postgis-$POSTGIS_VERSION
    createdb -E UTF8 template_postgis
    createlang -d template_postgis plpgsql
    psql -d template_postgis -f $POSTGIS_PATH/postgis.sql
    psql -d template_postgis -f $POSTGIS_PATH/spatial_ref_sys.sql

If you experience failing tests here are two tips:

1. Debug the project data by running TileMill with

    ./index.js --files=./test/fixtures/files/

2. Try clearing the cache of test data:

    rm -rf ./test/fixtures/files/

For more info see: http://postgis.net/docs/manual-1.5/ch02.html


# Documentation

TileMill documentation is kept in the gh-pages branch, which is independently managed and not merged with master.

TileMill's in-app reference available as the "Manual" (see below for syncing details) is a very small subset of docs for offline usage and is manually
sync'ed from the gh-pages branch.

To view all the TileMill documentation locally, first checkout the gh-pages branch:

    git checkout gh-pages

Then install Jekyll:

    sudo gem install jekyll

And run Jekyll:

    jekyll

Once Jekyll has started you should be able to view the docs in a browser at:

    http://localhost:4000/tilemill/


# Syncing manual

To sync the manual with gh-pages updates do:

    export TILEMILL_SOURCES=`pwd`
    cd ../
    git clone --depth=1 -b gh-pages https://github.com/mapbox/tilemill tilemill-gh-pages
    cd ${TILEMILL_SOURCES}
    export TILEMILL_GHPAGES=../tilemill-gh-pages
    rm -rf ${TILEMILL_SOURCES}/assets/manual
    mkdir -p ${TILEMILL_SOURCES}/assets/manual
    cp -r ${TILEMILL_GHPAGES}/assets/manual/* ${TILEMILL_SOURCES}/assets/manual/
    git add ${TILEMILL_SOURCES}/assets/manual/*
    rm -rf ${TILEMILL_SOURCES}/_posts/docs/reference
    mkdir -p ${TILEMILL_SOURCES}/_posts/docs/reference
    cp -r ${TILEMILL_GHPAGES}/_posts/docs/reference/* ${TILEMILL_SOURCES}/_posts/docs/reference/
    git add ${TILEMILL_SOURCES}/_posts/docs/reference/*
