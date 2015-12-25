

#How to install Nodejs 0.12.0 stable for Raspberry Pi
The problem is due to recent releases of Raspbian mis-identifying the ARM version so Node tries to build for V7 instead of V6. These two changes from this io.js thread sort it out.
```linux
#Download nodejs-0.12.0 for Raspberry Pi
wget https://s3-eu-west-1.amazonaws.com/conoroneill.net/wp-content/uploads/2015/02/node-v0.12.0-linux-arm-pi.tar.gz
tar -zxvf node-v0.12.0-linux-arm-pi.tar.gz
cd node-v0.12.0-linux-arm-pi
sudo cp -R * /usr/local/
```

#Nodejs Scan BLE [noble, library]
Ref: https://github.com/sandeepmistry/noble
##Prerequisites
###Linux 
- Kernel version 3.6 or above
- libbluetooth-dev
###Ubuntu/Debian/Raspbian
sudo apt-get install bluetooth bluez libbluetooth-dev libudev-dev

##Install
```linux
    npm install noble
```

###Example

ตัวอย่างการ Scan BLE โดยใช้ Nodejs
```Javascript
var noble = require('noble');
noble.on('stateChange', function(state){
    console.log('state : '+state);
     if (state === 'poweredOn') {
      noble.startScanning([], true);
    } else {
      noble.stopScanning();
    }
});

noble.on('discover', function(peripheral) {
    console.log('on -> discover: ' + peripheral.advertisement.localName);
});
```

###Peripheral discovered
```Javascript
peripheral = {
  id: "<id>",
  address: "<BT address">, // Bluetooth Address of device, or 'unknown' if not known
  addressType: "<BT address type>", // Bluetooth Address type (public, random), or 'unknown' if not known
  connectable: <connectable>, // true or false, or undefined if not known
  advertisement: {
    localName: "<name>",
    txPowerLevel: <int>,
    serviceUuids: ["<service UUID>", ...],
    manufacturerData: <Buffer>,
    serviceData: [
        {
            uuid: "<service UUID>"
            data: <Buffer>
        },
        ...
    ]
  },
  rssi: <rssi>
};

noble.on('discover', callback(peripheral));
```



