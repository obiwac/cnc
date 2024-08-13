# Linear rails

Bars have too much deflection and off-axis backlash, so I'm going with linear rails.

## Supply

For the most part, I think this is a component we can cheap out on, as it looks like clones are really 90% as good as the real deal at like 10% the price.
Apparently they are susceptible to burrs/gunk from the factory, so it's probably best to degrease them (high-proof 97% or so isopropanol works great as a solvent) and then to regrease them (using something viscous enough like 3-in-1).

### Grease

**TODO** Look into this some more!

I don't know much about this so far, I just know not to use WD-40 ;)
And also not to use particle grease, as it's bad for the bearings.

## Rail series, model, and load

I'm going with HG-series linear rails.
MG-series seem more oriented to low-load applications, like 3D printers.

Also, I'm going with 20 mm rails for no other reason than they seem to be the most common size for this kind of application.
The one exception to this is the Z-axis, which uses 15 mm rails.

C-load is probably sufficient for the X/Z-axes, but the Y-axis should probably use H-load (if clones of these are available, otherwise I think C-load isn't the end of the world).
**TODO** I should actually calculate this!

With respect to load, HIWIN claims their HG-series rails have equal load ratings in the radial, reverse radial, and lateral directions, so the orientation we mount them in shouldn't matter.

## Rail blocks

For HG-series 20 mm rails, only H-blocks are supported; since we're going with these, make sure that clones' "R"-blocks (interchangeable?) are actually H-blocks (HGH20/HGR20).

## Other considerations

- This is obvious but, when designing with these rails, don't forget to account for margins when deciding on length! That includes the rail margin itself, the block margin, and the attachment margin (if applicable).
- Rails require backing plates. They will rack if not properly supported. Also, if having two rails in parallel, it's probably best to have them on the same backing plate.

## Resources

- [HIWIN model name reference card.](https://motioncontrolsystems.hiwin.us/Asset/HG-Series-Catalog.pdf)
- [Tuning Up Cheap Chinese Linear Bearing (YouTube video).](https://youtu.be/c53sa46C1qQ)
