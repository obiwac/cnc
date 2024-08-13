# Ballscrews

Leadscrews and belts are finicky, and I don't wanna have to deal with the 1001 hacks to reduce backlash.
So ballscrews it is, even though they're hella expensive.

## Manufacturing and runout

The two main ways that ballscrews are manufactured are as follows:

- Cold-rolling: These ballscrews are made by "squishing" the screw into shape. This is cheap, results in very rigid ballscrews, but the downside is that they have a lot of radial runout (which is naturally amplified for longer ballscrews).
- Grinding: These ballscrews are made by grinding the screw into shape. This is very expensive as it's very time-consuming, but the upshot is that these ballscrews are extremely straight and accurate.

Most of the ungraded (i.e. cheap, i.e. what we would like) ballscrews are cold-rolled.
I think we can probably get away with cold-rolled ballscrews, and there are techniques which we can retrofit later on to mitigate the runout issue:

- [Improving Cheap Ball Screws, Hackaday.](https://hackaday.com/2021/02/22/improving-cheap-ball-screws/)
- [Wobble-X (YouTube video).](https://www.youtube.com/watch?v=4kQZnSzZHD0)

## Ballscrew model

The two things to consider for a ballscrew is the diameter of the screw and the pitch between threads.
The models which seem to be the most common are SFU1204's (12 mm diameter, 4 mm pitch) and SFU1210's (12 mm diameter, 10 mm pitch).

With regards to pitch, what you trade in precision and load (smaller pitch allows you to make the most out of each of your stepper's steps) you make up for in speed.

The Z-axis uses SFU1605's, but I think I'd like the X/Y-axes to use SFU1210's as I have a feeling 4 mm is going to be too slow.
This means that the resolution the CNC will be able to mill will be asymmetrical.
I don't know if that's really a problem.

**TODO** Do the math, do I really need a larger pitch? How much precision do I get given a 1.8 deg step?

## Nuts

The nut is what travels up and down the ballscrew and is where the small bearings go around the ballscrew and are recirculated.

The nut that the Z-axis was built with is a single-flange nut (looks like a FSI type 2 nut).
This also looks like the easiest nut type to machine for; you just need to drill a hole in a sufficiently large block of steel and then drill screwholes for the flange to attach to on the side.
Then you can just attach whatever you want to whatever side of that block (preferably the side with the flat-end of the flange if using an FSI type 2 nut).

I think an FSB nut is essentially the same thing as an FSI type 2 nut, just that its flange is completely round instead of having two parallel flat sides.

## Bearing blocks (screw ends)

These are the bearing blocks on each end of the ballscrew which support it and hold it in place.
The one used on the motor-side of the Z-axis is a BK12, and the one used on the other side is an BF12.

### Machining of the ballscrew ends themselves

Depending on the type of screw end you use and the type of coupling you're going to have with your ballscrew, there are different ends you can have.

For a BF12, which is a dead-end kind of screw end, you really only have one choice (which HIWIN refers to as "Standard Support Bearing").
For a BK12, you are obligated to have a coupling end (usually 15 mm from what I can tell).

These can have various kinds of keyways, but the Z-axis doesn't have one.
This doesn't look to be completely necessary if a clamping jaw is used; I don't think there's enough torque in the system to have any slippage if there's sufficient clamping force.

## Resources

- There was a good HIWIN datasheet on their ballscrews, but the [link](https://hiwin.us/wp-content/uploads/ballscrews.pdf) seems to be dead unfortunately.
- [HIWIN ballscrew main configurator.](https://motioncontrolsystems.hiwin.com/configurator/ballscrews-main-configurator) You can download the models for the ballscrews here.
