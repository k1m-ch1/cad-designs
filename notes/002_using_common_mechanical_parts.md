So, although mechanical parts are more well known and perhaps, easier to understand than the foundational building blocks used in electronics and computer science, knowing how to actually attach it and use it all together is still not entirely obvious. There are some "tricks of the trade" that will probably be useful patterns when trying to do different things with physical rigid bodies.

So let's first talk about the different types of mechanisms that selectively chooses the degrees of freedom it wants.

A rigid body has 6 degrees of freedom: x, y and z for rectilinear motion and roll, pitch and yaw for rotational motion.

The different types of mechanisms in this sense is then the different possible subset formed from by those 6 options, with the constraint that we state that the 3 rectilinear motion are identical, and the 3 rotational motion are identical.

So, this is identical to the problem of having 3 red balls, and 3 green balls; and counting or forming all of the possible subsets (perhaps also including the empty set as, a fastener)

So essentially, we can think of this as such:

- We can either pick 0, 1, 2 or 3 of red
- we can either pick 0, 1, 2 or 3 of green

As such, that amounts to $4 \times 4 = 16$ possibilities. In our case:

```
[]
['r']
['r', 'r']
['r', 'r', 'r']
['l']
['l', 'r']
['l', 'r', 'r']
['l', 'r', 'r', 'r']
['l', 'l']
['l', 'l', 'r']
['l', 'l', 'r', 'r']
['l', 'l', 'r', 'r', 'r']
['l', 'l', 'l']
['l', 'l', 'l', 'r']
['l', 'l', 'l', 'r', 'r']
['l', 'l', 'l', 'r', 'r', 'r']
```

So, let's see what kind of mechanisms these combinations mechanically represent:

- `[]`: that's a rigid body that's fastened to the frame itself
- `['r']`: revolute joint, or actively driven by a motor
- `['r', 'r']`: U joint, or actively driven by 2 motors with one rotating the other such that perpendicular or something.
- `['r', 'r', 'r']`: spherical joint, or 3 motors on top of one another I guess, which is kinda like almost a robotic arm.
- `['l']`: linear slider, can be actively driven using belts and motors like on 3d printers.
- `['l', 'r']`: cylindrical joint, can be actively driven by a linear slider that's being rotated by a motor, or motor on the head that's driven by a linear slider, depending on what you want. A differential drive robot also falls under this category.
- `['l', 'r', 'r']`: a linear slider attached to a u-joint, or something? A plane does this, although, it's like, non-holonomic or something.
- `['l', 'r', 'r', 'r']`: linear slider attached to a spherical joint?
- `['l', 'l']`: 2 linear sliders, like a CNC machine
- `['l', 'l', 'r']`: 2 linear sliders with something rotating? Mechanum wheel robots and swerve drive robots I guess.
- `['l', 'l', 'r', 'r']`: can't think of anything
- `['l', 'l', 'r', 'r', 'r']`: can't think of anything either
- `['l', 'l', 'l']`: 3d printers
- `['l', 'l', 'l', 'r']`: 3d printers with movable head or something? I guess a quadcopter does something similar.
- `['l', 'l', 'l', 'r', 'r']`: 3d printer base with like, a turret or something?
- `['l', 'l', 'l', 'r', 'r', 'r']`: satellite with flywheel, and also, propulsion on all 3 axis or something?

Well, either way, we only care about a few of these, because, some of them are just compositions of some other more basic mechanism. Right now, here are some basic stuff that probably form the foundation to a bunch of other mechanisms:

# Revolute joint

So, a revolute joint can be formed by taking a cylindrical joint, and restricting its rectilinear degree of freedom.

A cylindrical joint can be formed by having a cylinder rotating around another cylindrical shell. 

## Axel

In the case of like, an axel, the shaft can supported at the two opposite sides with like bearings or something. We however, require to limit the rectilinear degree of freedom, as such, we need a shaft collar.

But now, we have to deal with friction. Right now, there's rubbing between the inner and the outer cylindrical shell, or in the case of the rear wheel drive axel, rubbing between the shaft and the outer housing thing (which can be solved using bearings I guess), and also, rubbing between the shaft collar and the chasis or some kind of base. I suppose we have no choice but to resolve this using some sort of lubricant.

In the case of 3d printed parts, is plastic on plastic rubbing fine as long as I sand it first and also lubricate it? Like, for instance, is it ok for a 3d printed rear axel to have the shaft rotate around a plastic hole with lubrication? What about for the shaft collar? Can it rub on the plastic with a layer of lubricant in between them?

It seems fine, but I guess I'll have to try it out first to see whether it will work.

If we want more robustness, we can consider:

- bearings or bushing
- thrust bearings

I also have a question regarding bearings. So, do you rigidly fix the bearings onto the shaft, or do you allow the bearing to rotate around the shaft too, because, intuitively, I would imagine that we wouldn't want rubbing between the inner bearing and the shaft itself, because that would defeat the point of the bearing?

It seems like usually, you'd want to fix the inner cylinder of the bearing to the shaft like intuition tells you, and to do that, you'd use something like a bearing shoulder and a retainer or a simple press fit.

We can also use one of those e-clips that's used to hold the shaft of a typical drone motors in place.

But it seems like, even if the shaft slides around in the bearing, it still seems fine.

## Short revolute joints

Joints for like for a pendulum can be formed using a bearing that's press-fitted into a 3d printed part, and then the inner part can be attached either using a metal rod that's screwed in place, or some 3d printed rod that's screwed in place.

As for revolute joints for like, the steering of a car, it should be possible to just use m3 screws that's going through a heated-insert to keep it perpendicular to the base, and some nylock nuts at the top to lock it in place, with some lubricant to make it a bit smoother. Perhaps we'd also want some washers too.

## rectilinear motion

Inspired by 3d printing, this can be done by a v-slot, or some linear bearings.
