# Mcrouter [![Build Status](https://travis-ci.org/facebook/mcrouter.svg?branch=master)](https://travis-ci.org/facebook/mcrouter)

Mcrouter (pronounced mc router) is a memcached protocol router for scaling [memcached](http://memcached.org/)
deployments. It's a core component of cache
infrastructure at Facebook and Instagram where mcrouter handles almost
5 billion requests per second at peak.

Mcrouter is developed and maintained by Facebook.

See https://github.com/facebook/mcrouter/wiki to get started.

## Quick start guide

### New! Ubuntu package available

Currently, we support Ubuntu Bionic (18.04) amd64.
Here is how to install it:

Add the repo key:

    $ wget -O - https://facebook.github.io/mcrouter/debrepo/bionic/PUBLIC.KEY | sudo apt-key add

Add the following line to apt sources file /etc/apt/sources.list

    deb https://facebook.github.io/mcrouter/debrepo/bionic bionic contrib

Update the local repo cache:

    $ sudo apt-get update

Install mcrouter:s

    $ sudo apt-get install mcrouter


### Installing From Source

See https://github.com/facebook/mcrouter/wiki/mcrouter-installation for more
detailed installation instructions.

Mcrouter depends on [folly](https://github.com/facebook/folly), [wangle](https://github.com/facebook/wangle), [fizz](https://github.com/facebookincubator/fizz), and [fbthrift](https://github.com/facebook/fbthrift).

The installation is a standard autotools flow:

    $ autoreconf --install
    $ ./configure
    $ make
    $ sudo make install
    $ mcrouter --help

Alternatively, you can install dep and compile by: 
```
./mcrouter/mcrouter/scripts/install_ubuntu_18.04.sh $HOME/mcrouter-install/ -j4
export LD_LIBRARY_PATH=$HOME/mcrouter-install/install/lib
# sudo ln -s $HOME/mcrouter-install/install/bin/mcrouter /usr/local/bin/mcrouter
```

You can rebuild mcrouter (once changes some files) by: 
```
cd mcrouter/mcrouter
make -j16
```

You can rebuild the folly, fizz, etc by deleting mcrouter/scripts/.fmt-done and run: 
```
./mcrouter/mcrouter/scripts/install_ubuntu_18.04.sh $HOME/mcrouter-install/ -j4
```

Assuming you have a memcached instance on the local host running on port 5001, the simplest mcrouter setup is:
    
    $ cd mcrouter/mcrouter
    $ ./mcrouter \
        --config-str='{"pools":{"A":{"servers":["127.0.0.1:5001"]}},
                      "route":"PoolRoute|A"}' \
        -p 5000

```
# Using config file and setting verbosity to level-4. 
./mcrouter --config-file=../configs/random.json -p 5000 -v 4 --num-proxies 16 --remote-thread true  --thread-affinity true

# set thread-affinity to minimize the number of connections between clients and memcached server (this willl hurt performance). 
./mcrouter --config-file=../configs/random.json -p 5000 --num-listening-sockets 16 --num-proxies 16 --remote-thread true --thread-affinity true

# set max connections and tcp listen backlog from clients. 
./mcrouter --config-file=../configs/random.json -p 5000 --num-listening-sockets 16 --num-proxies 16 --remote-thread true --max-conns 409600 --tcp-listen-backlog 409600

# using embeded mode. 
./mcrouter --config-file=../configs/random.json -p 5000 --num-listening-sockets 16 --num-proxies 16 --max-conns 409600 --tcp-listen-backlog 409600

printf "set mykey 0 0 4\r\ndata\r\n" | nc localhost 5000
printf "get mykey\r\n" | nc localhost 5000
```
(nc is the GNU Netcat, http://netcat.sourceforge.net/)

## Features

+ Memcached ASCII protocol
+ Connection pooling
+ Multiple hashing schemes
+ Prefix routing
+ Replicated pools
+ Production traffic shadowing
+ Online reconfiguration
+ Flexible routing
+ Destination health monitoring/automatic failover
+ Cold cache warm up
+ Broadcast operations
+ Reliable delete stream
+ Multi-cluster support
+ Rich stats and debug commands
+ Quality of service
+ Large values
+ Multi-level caches
+ IPv6 support
+ SSL support

## Links

Documentation: https://github.com/facebook/mcrouter/wiki
Engineering discussions and support: https://www.facebook.com/groups/mcrouter

## License

Copyright (c) Facebook, Inc. and its affiliates.

Licensed under the MIT license:
https://github.com/facebook/mcrouter/blob/master/LICENSE
