# Electronics

## Power supplies (PSU's, aka inverters)

We need two separate power supplies because the spindle needs 3-phase power provided by a variable frequency drive (VFD).
This is fundamentally incompatible with the simple DC power that the steppers and the rest of the electronics use.

### Stepper PSU

**TODO** I don't yet have this, so I'll refrain from writing a lot of prose about it until I do.

### Spindle VFD PSU

The PSU for the spindle is a EV51S20015BX0 (EV51-series).
It comes in a kit with the spindle itself, and the (pretty bad) documentation for it can be downloaded on StepperOnline's [product page for the spindle](https://www.omc-stepperonline.com/en-au/220v-1-5kw-80x73x175-5mm-air-cooled-spindle-motor-and-2hp-1-5kw-7-0a-variable-frequency-drive-kit-vsk-asl1-5b).

It looks like there are 3 modes of communication with the VFD:

- The built-in keypad (which will probably only be used for testing).
- An RS-485 serial connection (over an RJ45 network port for some reason).
- A 10V analog input terminal, which seems to be intended for a dumb external keypad.

#### Serial connection

The pinout is defined as follows in the documentation, with pin 1 seeming to be the left side of the female RJ45 connector and pin 8 being the right side:

- 1: 485+: Does this mean RX?
- 2: 485-: Does this mean TX?
- 3: Reserved.
- 4: P24: No clue. I did have a clue at some point, but I forgot.
- 5: COM: Ground.
- 6: +12V: 12V rail (can be ignored, we don't use 12V anywhere else AFAIK).
- 7: COM: Ground.
- 8: +5V: 5V rail.

**TODO** I can't for the life of me figure out the documentation for the serial communication protocol used.
I guess this is a "try it out and you'll see" kind of situation.

#### Analog input terminal

The AI1 (analog input) pin seems to accept a range of 0 to 10V, which controls the frequency.

There are also S1, S2, S3, and S4 pins which I'm unsure how to use exactly but they're some kind of digital input which I'm assuming is to set settings.
There is a table called "Multi stage speed comparison table" which has bits for S4, S3, and S2 - maybe this is an alternative way of setting frequency?

There's also a bunch of "parameters" each with their own number, but it is totally unclear how exactly you're supposed to access them and the documentation seems incomplete because it self-refers to parameters it doesn't list anywhere.

Finally, there are RA and RB pins which are just labeled as "relay output" and "fault" and V or I settings jumpers which aren't explained anywhere from what I can tell but are placed close to AI1 in the "NO.2 Wiring diagram for carving machine inverter" diagram.

#### Input (wall) and output (spindle)

These are the row of terminals all the way at the bottom of the VFD.

The VFD is rated for the following input phases/voltages/frequencies:

- 3-phase 380V to 440V 50/60 Hz (using inputs R/L1, S/L2, and T/L3).
- 1-phase 200V to 240V 50/60 Hz (using inputs R/L1, S/L2).

I'm going with the 1-phase 230V 50 Hz option because that's what we have in Europe.
It claims it will also work fine with voltages up to 260V, which is good because sometimes the wall voltage passes 240V slightly at home.

It then outputs the 3-phase variable frequency power to the spindle through the U, V, and W terminals, which naturally need to be connected to the corresponding U, V, and W terminals on the spindle.

## Computer

The computer I'm going for is the Raspberry Pi Zero W, purely because I have like a dozen of them left over from a previous project.
Here are the relevant hardware features of the Raspberry Pi Zero:

- WiFi chip. This will allow the Raspberry to host a web interface which can be connected to to upload/monitor jobs. There could also be a button to put the Raspberry in AP mode so that a client can connect to it and enter the desired WiFi SSID and password without having to connect a keyboard and a display.
- VC4 GPU with a mini HDMI output. This could be used to display pretty job visualizations when connected to a monitor.
- Pretty crappy single-core armv6 (32-bit) 1 GHz CPU. I think this will be enough to generate simple toolpaths, host a web server, and the like though.
- 512 MB of RAM. Probably enough.
- 40-pin GPIO header. This will be detailed in the next section.
- OTG USB port with serial emulation. Can be used to hook up to a computer directly and offline with just a simple USB cable, for debugging, or offline monitoring and control.

The Raspberry will act as a daughterboard to a motherboard.
This will be presumably mean soldering a 40-pin male header to the GPIO and having an equivalent female header on the motherboard, and presumably a couple standoffs on the other side of the daughterboard to keep things solid.

### GPIO header

The GPIO header of the Raspberry Pi Zero W has 40 pins.
Here is how many are usable for which purposes and also what I want to use them for:

- 2 3.3V power: Seems pretty useless.
- 2 5V power: The PSU for the stepper motors has a 5V rail, so this can be used for power.
- 8 ground: I don't think I need to explain this lol.
- 28 GPIO's total.
- 4 GPIO's which support native I2C: Don't have a use for this.
- 2 GPIO's which support native UART: This is on top of the OTG USB serial emulation, so this could either be used for the VFD or for the stepper PSU if we end up using the analog terminal on the VFD. If we do use this for the VFD, we can always bit-bang on two of the other GPIO's for the stepper PSU.
- 5 GPIO's which support native SPI (0): Don't have a use for this.
- 6 GPIO's which support native SPI (1): Don't have a use for this.
- 4 GPIO's which support PWM (of which two overlap with SPI1): If it turns out I can use this as a speed control for the stepper speed controllers, this could be useful.
- 9 purely digital GPIO's: These can be used for limit switches and other communication.
