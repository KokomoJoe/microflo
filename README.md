MicroFlo: flow-based programming for microcontrollers
========================================================
[![Build Status](https://travis-ci.org/microflo/microflo.png?branch=master)](https://travis-ci.org/microflo/microflo)

Implementation of [Flow-based programming](http://en.wikipedia.org/wiki/Flow-based_programming)
for microcontrollers and embedded devices.
MicroFlo supports multiple targets, including [Arduino](http://arduino.cc), Atmel AVR,
ARM Cortex M devices (mbed, TI Tiva/Stellaris), ESP8266 and Embedded Linux.

Unlike most other visually programmable systems, MicroFlo programs runs _standalone_ on the microcontroller,
does not make use of code generation, can be introspected and reconfigured at runtime,
and supports automated testing.

One can program with MicroFlo either:

* Visually, using [Flowhub](https://flowhub.io)/[NoFlo UI](https://github.com/noflo/noflo-ui)
* Textually, using the declarative [.fbp DSL](http://noflojs.org/documentation/fbp)
or [.json definition](http://noflojs.org/documentation/json)
* Programatically, by embedding it and building a graph using the C++ API

MicroFlo is designed to integrate with other FBP runtimes,
like [NoFlo](http://noflojs.org/) and [msgflo](https://github.com)

Status
-------
**Minimally useful**.

* Simple programs work
* Components exists for most standard I/O on Arduino devices
* Minimal support for automated testing

MicroFlo in the wild:

* [Jon's fridge thermostat](http://www.jonnor.com/2013/09/microflo-0-1-0-and-an-arduino-powered-fridge/)
has been running uninterrupted since September 2013.
* The [Ingress table](http://bergie.iki.fi/blog/ingress-table/) at [c-base station](http://en.wikipedia.org/wiki/C-base), Berlin uses MicroFlo
to control the lights. One major upgrade/bugfix since March 2014.
* The lights on the CNC-milled Christmas tree at [Bitraf](http://bitraf.no),
Oslo ran for 4 weeks during Christmas 2013.

Contact
----------
Use the Google Group [Flow Based Programming](https://groups.google.com/forum/#!forum/flow-based-programming)
or IRC channel [#fbp@freenode.org](irc://fbp.freenode.org). Alternatively, file issues here on Github.


Milestones
-----------

[Past milestones](../CHANGES.md)

* September 2013, 0.1.0 "The Fridge". First deployment of a Microflo-based device
* November 2013, 0.2.0 "The start of something visual". First version programmable using Flowhub
* May 2014, 0.3.0 "Node in a Node, to infinity". Suppport for non-Arduino platforms, NoFlo integration

Future roadmap

* [0.4.0](https://github.com/microflo/microflo/issues?milestone=4)
* 0.5.0, "Simulated":
One can program and test MicroFlo programs in a simulator, before putting it on device
* 0.?.0, "Generally useful":
Most of Arduino tutorial have been, or can easily be, reproduced
* ?.0.0, "Production quality":
A commercial product based on MicroFlo has been shipped.
* ?.0.0, "Device freedom":
An open source hardware, free software electronics product based on MicroFlo with an integrated IDE
allowing and encouraging the user to customize the code has been shipped.


Using
-----------------
For visual programming your Arduino, follow the [microflo-example-arduino](https://github.com/microflo/microflo-example-arduino).

If interested in extending MicroFlo to support other microcontrollers, see the next section.


Developing/Hacking
-----------------

Instructions below valid for

* Arch Linux and Ubuntu (any GNU/Linux should be OK),
* Mac OSX 10.8 Mountain Lion (10.6 -> 10.9 should be OK)

You can however use the Arduino IDE or another tool for flashing your microcontroller.

Note: Mostly tested on Arduino Uno R3 and Arduino Nano R3. Other Arduino devices should however work.

Get the code

    git clone https://github.com/microflo/microflo.git
    cd microflo
    git submodule update --init # only for old git versions

Install prerequsites: Arduino 1.6 or later [Download](https://www.arduino.cc/en/Main/Software)

To build and run tests

    npm install && npm test

To flash your Arduino with the MicroFlo runtime, including an embedded graph:

    make upload GRAPH=examples/blink.fbp BOARD=arduino:avr:uno

Now you can use Flowhub Chrome app to talk directly to MicroFlo over serial/USB
or use:

    node microflo.js runtime

Then connect to the runtime (by default at http://localhost:3569) using Flowhub.
More details at the [getting started guide](http://flowhub.io/documentation/getting-started-microflo).

To see existing or add new components, check the files

* [./microflo/components.json](./microflo/components.json)
* [./microflo/components/](./microflo/components)

To see existing or add microcontroller targets, see

* [./microflo/main.hpp](./microflo/main.hpp)
* [./microflo/arduino.hpp](./microflo/arduino.hpp)

Remember to rebuild MicroFlo after changes:

    grunt build

When you find issues: [file bugs](https://github.com/microflo/microflo/issues)
and/or submit [pull requests](https://github.com/microflo/microflo/pulls)!

License
-------
MIT for the code in MicroFlo, see [./LICENSE](./LICENSE).

Note that some MicroFlo components may be under other licenses!

Goals
----------
1. People should not need to understand text-based,
C style programming to be able to program microcontrollers.
But those that do know it should be able to use that knowledge, and be able to mix-and-match it
with higher-level paradims within a single program.
2. It should be possible to verify correctness of a microcontroller program in an automated way,
and ideally in a hardware-independent manner.
3. It should be possible to visually debug microcontroller programs.
4. Microcontroller functionality should be reprogrammable on the fly.
5. Microcontrollers should easily integrate into and with higher-level systems:
other microcontrollers, host computers, and the Internet.
6. Microcontroller programs should be portable between different uC models and uC architectures.

Design
------
* Run on 8bit+ uCs with 32kB+ program memory and 1kB+ RAM
    * Primarily as bare-metal, but embedded Linux also possible in future
    * For initial component implementations, wrap Arduino/LUFA/etc
    * Components that are not tied to a particular I/O system shall have host-equivalents possible
* Take .fbp/.json flow definition files used by NoFlo as the canonical end-user input
    * Use NoFlo code to convert this to a more compact and easy-to-parse binary format, architecture-independent flow representation
    * Flows in binary format can be baked into the firmware image.
* Allow to introspect and manipulate graphs at runtime from a host over serial/USB/Ethernet.
    * Use a binary protocol derived from the binary graph representation for this.
* Allow a flow network to be embedded into an existing C/C++ application, provide API for manipulating
* Port and I/O configuration is stored in a central place, to easily change according to device/board deployed to.

Contributors
-------------
[Jon Nordby](http://www.jonnor.com/)


