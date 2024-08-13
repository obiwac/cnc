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
