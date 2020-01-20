# myopcserver
my first open62541 based OPC UA server for learning

## Build

Do following.

```sh
mkdir -p work/src
mkdir -p work/build
cd work/src
git clone https://github.com/nshimaza/myopcserver.git
git clone --recursive https://github.com/open62541/open62541.git
git clone https://github.com/OPCFoundation/UA-Nodeset.git
cd ../build
cmake ../src/myopcserver
make
```
