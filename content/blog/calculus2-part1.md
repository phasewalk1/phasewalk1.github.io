---
title: "Calculus II: Volumes"
date: 2022-03-05T11:29:09+01:00
draft: false
summary: Ways To Think About Slicing Methods
---

## Solids of Revolution

the first section of my Calculus II class is centered around integration. specifically, using integration methods to find the volume of some arbitrary shape. of course, we know how to trivially calculate volumes for novel shapes--things like cubes, cylinders, and pyramids. but how can we calculate the volume of shapes that form from bounded regions between two curves, and how do we do all this in only _two_ dimensions?

Calculus II sidesteps this problem of being a dimension short by a concept coined _Solids of Revolution_. these solids are true 3-dimensional objects, formed by rotating a 2-dimensional shape around a line or axis. this may seem confusing, considering we are still working on a 2-dimension plane; i've found that it helps me understand the concept better if i instead, imagine the _z_-axis as still being there, but that it's _invisible_ and thus, we do not need to define it explicitly nor worry about it at all.

## Integration Methods for Finding the Volume of a Solid of Revolution

### Disk and Washer

there are a handful of methods for evaluating the volume of a solid of revolution, though all methods employ the same concept of slicing the solid into infinitesimals. the most fundamental method being the _disk_/_washer_ method. We define the _disk_ method as such,

[$$ V = \pi\int_a^b f^2(x)\space dx $$]
and the _washer_ method as so,

[$$ \underbrace{V=\pi\int_a^b R^2_o(x) - R^2_i(x) \space dx}_{\text{where }R_o\text{ is the top function and }R_i\text{ the bottom function}} $$]

we group together these two methods because, fundamentally, they are the same. we can simply think of the _disk_ method as the _washer_ method under the hood, just that it always has an inner radius [$ R_i $] equivalent to [$ 0 $].

> > Note: both the _disk_ and _washer_ methods integrate with respect to the axis of rotation. i.e., when rotating about the _x_-axis, we integrate _dx_.

### Cylindrical Shells

some _solids of revolution_ can be tricky to evaluate with the _disk_/_washer_ method, and thus, we look to a different method: **cylindrical shells**. this method can't really be grouped with the others, because it is implemented using _cylinders_ instead of disks or washers, and thus, requires a function to represent the cylinder's height during integration.

#### Finding the Height Function

recall how, in the _washer_ method, we subtract the top function [$ R_o $] from the bottom function [$ R_i $]. we find the height function used in **cylindrical shells** similarly,

[$$ h(x)=f(x)-g(x) $$]
where [$ f(x) $] is the top function, and [$ g(x) $] is the bottom function.

#### Finding the Radius Function

now with a function that describes the shells _height_ as we move through the solid, we must find its _radius_. if the axis of rotation is simply an axis on the coordinate system, e.g., say we're rotating about the _y_-axis, our radius will simply be [$ R(x)=x $]. however, if we're rotating about some arbitrary line that does not fall on an axis, say, [$ x=5 $], and our solid lays on the left-side of that line, we must account for that offset of our radius, and thus [$ R(x) = (5-x) $].

#### Tying it All Together

finally, we have two functions. one that describes our cylindrical shell's radius as it moves through the solid, and one that describes our shell's height. we can now evaluate the solid's volume by

[$$ V = 2\pi\int_a^b R(x)h(x)\space dx $$]
