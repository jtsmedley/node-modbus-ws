# node-modbus-ws
NodeJS Modbus to WebSocket bridge

[![NPM Version](https://img.shields.io/npm/v/gm.svg?style=flat)](https://www.npmjs.com/package/modbus-ws)

Control your modbus enabled arduino project, toaster or robot via web browser.

The modbus-ws server allows a browser to connect to a modbus device, using websockets.
When the server is running, and connected to Serial line or Ethernet, 
a web browser can send web socket requests and control a modbus device.

With this server you can build web pages that will easily monitor and send requests to your modbus project or robot.

- [node-modbus-ws](#node-modbus-ws)
      - [Install](#install)
      - [Serial port support](#serial-port-support)
      - [Start the modbus-ws server](#start-the-modbus-ws-server)
      - [Server help](#server-help)
      - [Client side code](#client-side-code)
      - [Examples](#examples)

#### Install
Install the server:
```
npm install modbus-ws -g
```

This will add the **modbus-ws** command to your path. After install, you can run the server by typing **modbus-ws** on the command line.

[ If install locally, run the server using the **modbus-ws.js** file in the bin directory. ]

#### Start the modbus-ws server

Run the server using a serial port or a tcp connection:
```
modbus-ws -s /dev/ttyUSB0
modbus-ws -i 192.168.1.42
```

#### Server help
```
modbus-ws --help
```

```
  Usage: modbus-ws [options]

  Options:

    -h, --help              output usage information
    -V, --version           output the version number
    -s, --serial <port>     Use serial port, set port. [false]
    -b, --baudrate <boud>   Set serial port baudrate. [9600]
    -i, --ip <ip>           Use tcp/ip, set slave url or ip address. [false]
    -P, --tcpport <number>  Server port number [3000]
    -c, --nocache           Do not use caching for modbus comunication. [false]
    -w, --nohttp            Run only websocket server, no httpd. [false]

  Examples:

    modbus-ws --ip 192.168.1.24
       create a bridge to a modbus slave using tcp/ip.
    modbus-ws --serial /dev/ttyUSB0
       create a bridge to a modbus slave using a serial port.
    modbus-ws
       when serial and tcp/ip are not used, default to test.
       create a bridge to a modbus simulated slave.
    modbus-ws --ip 192.168.1.24 --nocache
       create a bridge with modbus without cache.
    modbus-ws --ip 192.168.1.24 --nocache --nohttp
       create a bridge with modbus without cache and without http web server.
```

#### Client side code

###### Get the socket.io code:
```
<script src="https://cdn.socket.io/socket.io-1.3.7.js"></script>
```

###### Use socket.io events
```javascript
// connect to sever
var socket = io("ws://127.0.0.1:3000/");

// set up socket.on for data received from sever
// server trigger 'data' event when data is received from device.
socket.on('data', function(data){
    console.log('received:', data);
    
    ... do something fun and interesting with data ...
});

// ask server to get registers
// "Read Input Registers" (FC=04) 
socket.emit("getRegisters", {
    "unit": 1,
    "address": 0,
    "length": 10
});

// subscribe to get registers every 1000ms
// "Read Input Registers" (FC=04) 
socket.emit("getRegisters", {
    "unit": 1,
    "address": 0,
    "length": 10,
    "interval": 1000
});

// ask server to set registers
// "Preset Multiple Registers" (FC=16)
socket.emit('setRegisters', {
    "unit": 1,
    "address": 8,
    "values": [88,123,47]
});
```

#### Examples

When the server is running it will transfer web socket requests into modbus requests and return the replays received as web socket events.

See the examples directory in the server's code tree, load an example file to a web browser, and watch for the data received from server.

