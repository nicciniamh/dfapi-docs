# DucksFeet API Server Shell Wrapper

The DucksFeet API uses a server script to start the server on each node. The wrapper is called *rapi-server* and the readon for this is, simply, the name was one 'remote api'. 

The wrapper script supports serveral flags to allow for inplace testing and debugging without disturbing a running instance. 

## Usage

rapi-server flags

| Flag | Purpose |
|------|---------|
|-d, --work-dir   *dir*   | Use *dir* as the working directory. Default /opt/rapi|
|-i, --ini-file   *file*  | Use *fie* as ini-file. Default server.ini|
|-v, --log-level  *level* | Set log level to INFO WARN ERROR FATAL DEBUG. Default INFO|
|-h, --help               | Show this message and exit.|
