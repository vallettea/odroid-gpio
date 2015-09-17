odroid-gpio
=======

odroid-gpio is a simple node.js based library to help access the GPIO of the Odoid C1. 

```javascript
var gpio = require("odroid-gpio");

gpio.open(16, "output", function(err) {		// Open pin 16 for output
	gpio.write(16, 1, function() {			// Set pin 16 high (1)
		gpio.close(16);						// Close pin 16
	});
});
```

## Pin configuration

You can use the physical numbering:

![Odroid pins](http://cdn.overclock.net/b/b6/b6755fac_ss2014-12-27at05.02.37.png)

## Requirement: gpio-admin

The GPIO pins require you to be root to access them. That's totally unsafe for several reasons. To get around this problem, you should use the excellent [gpio-admin](https://github.com/quick2wire/quick2wire-gpio-admin).

Do the following on your raspberry pi:

	git clone git://github.com/quick2wire/quick2wire-gpio-admin.git
	cd quick2wire-gpio-admin
	make
	sudo make install
	sudo adduser $USER gpio

replaceing $USER your username.

After this, you will need to logout and log back in. 

## Usage

### .open(pinNumber, [options], [callback])

Aliased to ``.export``

Makes ``pinNumber`` available for use. 

* ``pinNumber``: The pin number to make available. Remember, ``pinNumber`` is the physical pin number on the Pi. 
* ``options``: (Optional) Should be a string, such as ``input`` or ``input pullup``. You can specify whether the pin direction should be `input` or `output` (or `in` or `out`). You can additionally set the internal pullup / pulldown resistor by sepcifying `pullup` or `pulldown` (or `up` or `down`). If options isn't provided, it defaults to `output`. If a direction (`input` or `output`) is not specified (eg. only `up`), then the direction defaults to `output`.
* ``callback``: (Optional) Will be called when the pin is available for use. May receive an error as the first argument if something went wrong.

### .close(pinNumber, [callback])

Aliased to ``.unexport``

Closes ``pinNumber``.

* ``pinNumber``: The pin number to close. Again, ``pinNumber`` is the physical pin number on the Pi.
* ``callback``: (Optional) Will be called when the pin is closed. Again, may receive an error as the first argument.

### .setDirection(pinNumber, direction, [callback])

Changes the direction from ``input`` to ``output`` or vice-versa.

* ``pinNumber``: As usual.
* ``direction``: Either ``input`` or ``in`` or ``output`` or ``out``.
* ``callback``: Will be called when direction change is complete. May receive an error as usual.

### .getDirection(pinNumber, [callback])

Gets the direction of the pin. Acts like a getter for the method above.

* ``pinNumber``: As usual
* ``callback``: Will be called when the direction is received. The first argument could be an error. The second argument will either be ``in`` or ``out``. 

### .read(pinNumber, [callback])

Reads the current value of the pin. Most useful if the pin is in the ``input`` direction.

* ``pinNumber``: As usual.
* ``callback``: Will receive a possible error object as the first argument, and the value of the pin as the second argument. The value will be either ``0`` or ``1`` (numeric).

Example:
```javascript
gpio.read(16, function(err, value) {
	if(err) throw err;
	console.log(value);	// The current state of the pin
});
```

### .write(pinNumber, value, [callback])

Writes ``value`` to ``pinNumber``. Will obviously fail if the pin is not in the ``output`` direction.

* ``pinNumber``: As usual.
* ``value``: Should be either a numeric ``0`` or ``1``. Any value that isn't ``0`` or ``1`` will be coerced to be boolean, and then converted to 0 (false) or 1 (true). Just stick to sending a numeric 0 or 1, will you? ;)
* ``callback``: Will be called when the value is set. Again, might receive an error.

## Misc

* To run tests: ``npm install && npm test`` where you've got the checkout.
* This module was created, ``git push``'ed and ``npm publish``'ed all from the Raspberry Pi! The Pi rocks!

## Inspiration

This lib is inspired by [pi-gpio](https://github.com/rakeshpai/pi-gpio).

MIT License
