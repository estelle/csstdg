# X

# Transforms

introduction

# Coordinate Systems

Before embarking on this journey, let's take a moment to orient ourselves. There are two types of coordinate systems that are used in transforms, and it's a good idea to be familiar with both. (If you're already well familiar with Cartesian and spherical coordinate systems, particularly as used in computing, feel free to skip to the next section.)

The first is the Cartesian coordinate system, or what's often called the X/Y/Z coordinate system. This system is a way of describing the position of a point in space using two numbers (for two-dimensional placement) or three numbers (for three-dimensional placement). In CSS, the system uses three axes: the X or horizontal axis, the Y or vertical axis, and the Z or depth axis. This is illustrated in Figure 1.

> [[ Figure 1. The three Cartesian axes used in CSS transforms. ]]

For any 2D (two-dimensional) transform, you only need to worry about the X and Y axes. By convention, positive X values go to the right, and negative to the left. Similarly, positive Y values go downward along the Y axis, while negative values go upward. That might seem a little weird, since we tend to think that higher numbers should place something higher up, not lower down. If it helps to remember, you can visualize this as being the same as the X axis, just rotated a quarter-turn clockwise. It's also the same behavior seen with the `top` property in positioning, if you're used to that.

Given this, in order to move an element leftward and down, you would give it a negative X and a positive Y value, like this:

	translateX(-5em) translateY(33px)

That is in fact a valid transform value, as we'll see in just a bit. Its effect is to translate (move) the element five ems to the left, and 33 pixels down.

If you want to transform something in three-dimensional space, then you add a Z-axis value. This axis is the one that "sticks out" of the display and runs straight through your head. In a theoretical sense, that is. Positive Z values are closer to you, and negative Z values are further away from you. In this regard, it's exactly like the `z-index` property.

So let's say that we want to take the element we moved before and add a Z-axis value.

	translateX(-5em) translateY(33px) translateZ(200px)

So now the element will appear 200 pixels closer to us than it would be without the Z value.

Well you might wonder exactly how an element can be moved 200 pixels closer to you, given that holographic displays are regrettably rare and expensive. How many molecules of the air between you and your monitor is equivalent to 200 pixels? What does an element moving closer to you even look like, and what happens if it gets _too_ close? These are excellent questions that we'll get to later on. For now, just accept that moving an element along the Z axis appears to move it closer or further away.

The really important thing to remember is that every element carries its own frame of reference, and so considers its axes with respect to itself. That is to say, if you rotate an element, the axes rotate along with it, as illustrated in Figure 2. Any further transforms are calculated with respect to those rotated axes, not the axes of the display.

> [[ Figure 2. Elemental frames of reference. ]]

Speaking of rotations, the other coordinate system used in CSS transforms is a spherical system, which describes angles in 3D space. It's illustrated in Figure 3.

> [[ Figure 3. The spherical coordinate system used in CSS transforms. ]]

For purposes of 2D transforms, you only have to worry about a single 360-degree polar system: the one that sits on the plane described by the X and Y axes. This actually describes rotations around the Z axis, since that's the axis that runs through the center of the circle like the axle of a wheel. Similarly, rotations around the X axis tilt the element toward or away from you, and rotations around the Y axis turn the element from side to side. These are illustrated in Figure 4.

> [[ Figure 4. Rotations around the three axes. ]]

But back to 2D rotations. Suppose you wanted to rotate an element 45 degrees clockwise in the plane of the display (i.e., around the Z axis). The transform value is:

	rotateZ(45deg)

Change that to `–45deg`, and the element will rotate counter-clockwise (anti-clockwise for our international friends) around the Z axis. In other words, it will rotate in the X/Y plane, as illustrated in Figure 5.

> [[ Figure 5. Rotations in the XY plane. ]]

You can use degree to define skews, which will be covered later, but most authors will use degrees for rotational purposes.

All right—now that we have our bearings, let's get started with transforms!

# Transforming

There's really only one property that applies transforms, along with a few ancillary properties that affect exactly how the transforms are applied. We'll start with the Big Cheese.

---

`transform`

 **Values:**

	<transform-list> | none | inherit

 **Initial value:**

	none

 **Applies to:**

All elements except "atomic inline-level" boxes (see explanation)

 **Inherited:**

No

 **Percentages:**

Refer to the size of the bounding box (see explanation)

 **Computed value:**

As specified, except for relative length values, which are converted to an absolute length

---

First off, let's clear up the matter of the bounding box. For any element being affected by CSS, this is the border box; that is, the outermost edge of the element's border. That means that any outlines and margins are ignored for the purposes of calculating the bounding box. Special note: if a table-display element is being transformed, its bounding box is the table wrapper box, which encloses the table box and any association caption box.

If you're transforming an SVG element with CSS, then its bounding box is its SVG-defined _object bounding box_. Simple enough!

Now, the value entry *<transform-list>* requires some explanation. This placeholder refers to a list of one or more transform functions, one after the other, in space-separated format. It looks like this, with the result shown in Figure 6:

	#example {transform: rotate(30deg) skewX(-25deg) scaleY(2);}

> [[ Figure 6. A transformed div element. ]]

The functions are processed one at a time, starting with the first (leftmost) and proceeding to the last (rightmost). This first-to-last processing order is important, because changing the order can lead to drastically different results. Consider the following two rules, which have the results shown in Figure 7.

	img#one {transform: translateX(200px) rotate(45deg);}
<<<<<<< HEAD
<<<<<<< HEAD
	
=======
>>>>>>> eric
=======
>>>>>>> eric
	img#two {transform: rotate(45deg) translateX(200px);}

> [[ Figure 7. Different transform lists, different results. ]]

In the first instance, an image is translated (moved) 200 pixels along its X axis, and then rotated 45 degrees. In the second instance, an image is rotated 45 degrees and then moved 200 pixels along its X axis—that's the X axis of the transformed element, _not_ of the parent element, page, or viewport. In other words, when an element is rotated, its X axis (along with its other axes) rotates along with it. All element transforms are conducted with respect to the element's own frame of reference.

Compare this to a situation where an element is translated and then scaled, or vice versa; it doesn't matter which is which, because the end result is the same.

	img#one {transform: translateX(100px) scale(1.2);}
<<<<<<< HEAD
<<<<<<< HEAD
	
=======
>>>>>>> eric
=======
>>>>>>> eric
	img#two {transform: scale(1.2) translateX(100px);}

The situations where the order doesn't matter are far outnumbered by the situations where it does, so in general it's a good idea to just assume the order always matters, even when it technically doesn't.

Note that when you bave a series of transform functions, all of them must be properly formatted; that is, valid. If even one function is invalid, it renders the entire value invalid. Consider:

	img#one {transform: translateX(100px) scale(1.2) rotate(22);}

Because the value for `rotate()` is invalid—rotational values must have a unit—the entire value is dropped. The image in question will simply sit there in its initial untransformed state, neither translated nor scaled, let alone rotated.

It's also the case that transforms are not cumulative. That is to say, if you apply a transform to an element and then later want to add a transformation, you need to restate the original transform. Consider the following scenarios, illustrated in Figure XX:

	#ex01 {transform: rotate(30deg) skewX(-25deg);}
	#ex01 {transform: scaleY(2);}
	#ex02 {transform: rotate(30deg) skewX(-25deg);}
	#ex02 {transform: rotate(30deg) skewX(-25deg) scaleY(2);}

> [[ Figure XX. Overwriting or modifying transforms. ]]

In the first case, the second rule completely replaces the first, meaning that the element is only scaled. This actually makes some sense; it's the same as if you declare a font size and then elsewhere declare a different font size for the same element. You don't get a cumulative font size that way. You just get one size or the other. In the second example, the entirety of the first set of transforms was included in the second set, so they all got applied along with the scaleY() function.

There is an exception to this, which is that animated transforms, whether using transitions or actual animations, _are_ additive. That way, you can take an element that's transformed and then animate one of its transform functions without overwriting the others. For example, assume you had:

	img#one {transform: translateX(100px) scale(1.2);}

If you then animate the element's rotation angle, it will rotate from its translated, scaled state to the new angle, and its translation and scale will remain in place.

What makes this interesting is that even if you don't explicitly specify a transition or animation, you can still create additive transforms via the user-interaction psuedo-classes, such as `:hover`. That's because things like hover effects are types of transitions—they're just not invoked using the transition properties. Thus, you could declare:

	img#one {transform: translateX(100px) scale(1.2);}
	img#one:hover {transform: rotate(-45deg);}

This would rotate the translated, scaled image 45 degrees to its left on hover. The rotation would take place over zero seconds, because no transition interval was declared, but it's still an implicit transition. Thus, any state change can be thought of as a transition, and thus any transforms that are applied as a result of those state changes are additive with previous transforms.

<<<<<<< HEAD
> As of mid-2014, `transform` still had to be vendor-prefixed in WebKit and Blink browsers like Safari and Chrome. No prefixes were needed in other major user agents.
=======
> As of mid-2014, transform still had to be vendor-prefixed in WebKit and Blink browsers like Safari and Chrome. No prefixes were needed in other major user agents.
>>>>>>> eric

## The transform functions

There are, as of this writing, 21 different transform functions, employing a number of different value patterns to get their jobs done. Table X provides a list of all the available transform functions, minus their value patterns.

> Table X. Transform functions.

<table>
<tr>
<td>
translate()<br>
translate3d()<br>
translateX()<br>
translateY()<br>
translateZ()
</td><td>
scale()<br>
scale3d()<br>
scaleX()<br>
scaleY()<br>
scaleZ()
</td><td>
rotate()<br>
rotate3d()<br>
rotateX()<br>
rotateY()<br>
rotateZ()
</td><td>
skew()<br>
skewX() <br>
skewY() <br>
matrix()<br>
matrix3d()<br>
perspective()
</td>
</tr>
</table>


As previously stated, the most common value pattern for transform is a space-separated list of one or more functions, processed from first (leftmost) to last (rightmost), and all of the functions must have valid values. If any of the functions is invalid, it will invalidate the entire value of transform, thus preventing any transformation at all.

### Translation functions

A translation transform is just a move along one or more axes. For example, `translateX()` moves an element along its own X axis, `translateY()` moves it along its Y axis, and `translateZ()` along its Z axis.

Functions

	translateX(), translateY()

Permitted values

	<length> | <percentage>

These are usually referred to as the "2D" translation functions, since they can slide an element up and down or side to side, but not forward or backward along the Z axis. Each of these accepts functions a single distance value, expressed as either a length or a percentage.

If the value is a length, then the effect is about what you'd expect. Translate an element 200 pixels along the X axis with `translateX(200px)`, and it will move 200 pixels to its right. Change that to `translateX(-200px)`, and it will move 200 pixels to its left. For `translateY()`, positive values move the element downward; negative values move it upward, both with respect to the element itself. Thus, if you flip the element upside-down by rotation, positive `translateY()` values will actually move the element downward on the page.

If the value is a percentage, then the distance is calculated as a percentage of the element's own size. Thus, `translateX(50%)` will move a 300-by-300—pixel element to its right by 150 pixels, and `translateY(-10%)` will move that same element upward (with respect to itself) by 30 pixels.

Function

	translate()

Permitted values

	[ <length> | <percentage> ] [, <length> | <percentage>]?

If you want to translate an element along both the X and Y axes at the same time, then `translate()` makes that simple. Just supply the X value first and the Y value second, and it will act the same as if you combined `translateX() translateY()`. If you omit the Y value, then it's assumed to be zero. Thus, `translate(2em)` is treated as if it were `translate(2em,0)`, which is also the same as `translateX(2em)`. See Figure XX for some examples of 2D translation.

> [[ Figure XX. Translating in two dimensions. ]]

>According to the latest version of the specification, both the 2D translation functions can be given a unitless number. In that case, the number is treated as being expressed in terms of a "user unit,"which is treated the same as a pixel unless otherwise defined. The CSS specification does not explain how a user unit is otherwise defined: the SVG specification does, albeit briefly. In the field, no browser tested as of this writing supported unitless numbers of translation values, so the capability is academic at best.

Functions

	translateZ()

Permitted value

	<length>


This function translates elements along the Z axis, thus moving them into the third dimension. Unlike the 2D translation functions, `translateZ()` only accepts length values. Percentage values are _not_ permitted for `translateZ()`, or indeed for any Z-axis value.

Functions

	translate3d()

Permitted values

	[ <length> | <percentage> ], [ <length> | <percentage>], [ <length> ]

Much like `translate()` does for X and Y translations, `translate3d()` is a shorthand function that incorporates the X, Y, and Z translation values into a single function. This is obviously handy if you want to move an element over, up, and forward in one fell swoop. See Figure XX for some examples of 3D translations.

> [[ Figure XX. Translating in three dimensions. ]]

Unlike `translate()`, there is no fallback for situations where `translate3d()` does not contain three values. Thus, `translate3d(1em,-50px)` should be treated as invalid by user agents instead of being assumed to be `translate3d(2em,-50px,0)`.

### Scale functions

A scale transform makes an element larger or smaller, depending on what value you use. These values are unitless real numbers, and are always positive. On the 2D palne, you can scale along the X and Y axes individually, or scale them together.

Functions

	scaleX(), scaleY(), scaleZ()

Permitted value

	<number>


The number value supplied to a scale function is a multipler; thus, `scaleX(2)` will make an element twice as wide as it was before the transformation, whereas `scaleY(0.5)` will make it half as tall. Given this, you might expect that percantage values are permissible as scaling values, but they aren't.

Function

	scale()

Permitted value

	<number> [, <number>]?

If you want to scale along both axes simultaneously, use scale(). The X value is always first and the Y always second, so `scale(2,0.5)` will make the element twice as wide and half as tall as it was before being transformed. If you only supply one number, it is used as the scaling value for both axes; thus, scale(2) will make the element twice as wide _and _twice as tall. This is in contrast to `translate()`, where an omitted second value is always set to zero. `scale(1)` will scale an element to be exactly the same size it was before you scaled it, as will scale(1,1). Just in case you were dying to do that.

Of course, if you can scale in two dimensions, you can also scale in three. CSS offers `scaleZ()` for scaling just along the Z axis, and `scale3d()` for scaling along all three axes at once.

Function

	scale3d()

Permitted value

	<number>, <number>, <number>

Similar to `translate3d()`, `scale3d()` requires all three numbers to be valid. If you fail to do this, then the malformed `scale3d() `will invalidate the entire transform value to which is belongs. See Figure XX for some examples of element scaling.

> [[Figure XX. Scaled elements.]]


### Rotation functions

A rotation function causes an element to be rotated around an axis, or around an arbitrary vector in 3D space.  There are four simple rotation functions, and one less simple function meant specifically for 3D.

Functions

	rotate(), rotateX(), rotateY(), rotateZ()

Permitted values

	<angle>


All four basic rotation functions accept just one value, a degree.  This can be expressed using any of the valid degree units (`deg`, `grad`, `rad`, and `turn`) and a number, either positive or negative.

If a value’s number runs outside the usual range for the given unit, it will be normalized to fit into the accepted range.  In other words, a value of `437deg` will be tilted the same as if it were `77deg`—or, for that matter, `-283deg`.  Note, however, that these are only exactly equivalent if you don’t animate the rotation in some fashion.

That is to say, animating a rotation of `1100deg` will spin the element around several times before coming to rest at a tilt of -20 degrees (or 340 degrees, if you like).  By contrast, animating a rotation of `-20deg` will tilt the element a bit to the left, with no spinning; and of course animating a rotation of `340deg` will animate an almost-full spin to the right.  All three animations come to the same end state, but the process of getting there is very different in each case.

The function `rotate()` is a straight 2D rotation, and the one you’re most likely to use.  It is equivalent to `rotateZ()` because it rotates the element around the Z axis (the one that shoots straight out of your display and through your eyeballs).  In a like manner, `rotateX()` causes rotation around the X axis, thus causing the element to tilt toward or away from you; and `rotateY()` rotates the elmeent around its Y axis, as though it were a door.  These are all illustrated in Figure XX.

> [[Figure XX. Rotations around the three axes.]]


Function

	rotate3d()

Permitted value

	<number>, <number>, <number>, <angle>

If you’re comfortable with vectors and want to rotate an element through 3D space, then `rotate3d()` is for you.  The first three numbers specify the X, Y, and Z components of a vector in 3D space, and the degree value determines the amount of rotation around the declared 3D vector.

To start with a simple example, the 3D equivalent to `rotate(45deg)` is `rotate3d(0,0,1,45deg)`.  That specifies a vector of zero magnitude on the X and Y axes, and a magnitude of 1 along the Z axis.  In other words, it describes the Z axis.  The element is thus rotated 45 degrees around that vector, as shown in Figure XX.  That figure also shows the appropriate `rotate3d()` values to rotate an element by 45 degrees around the X and Y axes.
<<<<<<< HEAD

> [[Figure XX. Rotations around 3D vectors.]]

=======

> [[Figure XX. Rotations around 3D vectors.]]

>>>>>>> eric
A little more complicated is something like `rotate3d(-0.25,0.35,0.66667,45deg)`, where the described vector points off in to 3D space between the axes.  This has the result shown (and illustrated via schematic) in Figure XX.

> [[Figure XX. Rotation around a 3D vector, and how that vector is determined.]]

If you’re no comfortable with vectors, that’s okay; most people aren’t.  You’ll really only ever need to use them if you’re doing precise 3D calculations, at wwhich point you’ll be getting very familiar with vectors whether you want to or not.

### Skew functions

When you skew an element, you slant it along one or both of the X and Y axes.  There is no Z-axis or other 3D skewing.

Functions

	skewX(), skewY()

Permitted value

<<<<<<< HEAD
<<<<<<< HEAD
much math here (yikes)
=======
	<angle>

In both cases, you supply an angle value, and the element is skewed to match that angle.  It’s much easier to show skewing rather than try to explain it in words, so Figure XX shows a number of skew examples along the X and Y axes.

> [[Figure XX. Skewing along the X and Y axes.]]


Function

	skew()

Permitted values

	<angle> [, <angle> ]?


The `skew()` function is just a shorthand for `skewX()` and `skewY()`, accepting either one degree value or two comma-separated degree values.  If you have two values, the X skew angle is always first, and the Y skew angle comes second.  If you leave out a Y skew angle, then it’s treated as zero.  This means `skew(45deg)` is functionally equivalent to `skewX(45deg)`.  Figure XX shows some double-skewed elements.
>>>>>>> eric
=======
	<angle>

In both cases, you supply an angle value, and the element is skewed to match that angle.  It’s much easier to show skewing rather than try to explain it in words, so Figure XX shows a number of skew examples along the X and Y axes.

> [[Figure XX. Skewing along the X and Y axes.]]


Function

	skew()

Permitted values

	<angle> [, <angle> ]?


The `skew()` function is just a shorthand for `skewX()` and `skewY()`, accepting either one degree value or two comma-separated degree values.  If you have two values, the X skew angle is always first, and the Y skew angle comes second.  If you leave out a Y skew angle, then it’s treated as zero.  This means `skew(45deg)` is functionally equivalent to `skewX(45deg)`.  Figure XX shows some double-skewed elements.

>>>>>>> eric

> [[Figure XX. Skewed elements.]]

> [[Figure XX. Skewed elements.]]


### The perspective function

If you’re transforming an element in 3D space, you most likely want it to have some perspective.  Perspective gives the appearance of front-to-back depth, and you can vary the amount of perspective applied to an element.

### The perspective function

If you’re transforming an element in 3D space, you most likely want it to have some perspective.  Perspective gives the appearance of front-to-back depth, and you can vary the amount of perspective applied to an element.

Function

	perspective()

Permitted values

	<length>

It might seem a bit weird that you specify perspective as a distance.  After all, `perspective(200px)` seems a bit odd when you can’t really measure pixels along the Z axis.  And yet, here we are.  You supply a length, and the illusion of depth is constructed around that value.  Lower numbers create more extreme perspective, as though you were right up close to the element and viewing it through a fisheye lens.  Higher numbers create a gentler perspective, as though viewing the element through a zoom lens from far away.  _Really_ high perspective values create an isometric effect.

This makes a certain amount of sense.  If you visualize perspective as a pyramid, with its apex point at the perspective origin and its base the closest thing to you, then a shorter distance between apex and base will create a shallower pyramid, and thus a more extreme distortion.  This is illustrated in Figure XX.

> [[ Figure XX. Different perspective pyramids. ]]

In the documentation for Safari, Apple writes that perspective values below `300px` tend to be extremely distorted, values above `2000px` create “very mild” distortion, and values between `500px` and `1000px` create “moderate perspective.” [^1]  To illustrate this, Figure XX shows a series of elements with the exact same rotation as displayed with varying perspective values.

[^1]: https://developer.apple.com/library/safari/documentation/InternetWeb/Conceptual/SafariVisualEffectsProgGuide/Using2Dand3DTransforms/Using2Dand3DTransforms.html

> [[ Figure XX. The effects of varying perspective values. ]]

Perspecitve values must always be positive, non-zero lengths.  Any other value will cause the perspective to be ignored.

> !! Note that `perspective()` is very similar to the property `perspective`, which will be covered later, but they are applied in critically different ways.


### Matrix functions



much math here (yikes)



# More Transform Properties

various properties

## Moving the Origin

---

`transform-origin`

 **Values:**

	  [ left | center | right | top | bottom | <percentage> | <length> ]
	|
	  [ left | center | right | <percentage> | <length> ]
	  [ top | center | bottom | <percentage> | <length> ] <length>?
	|
	  [ center | [ left | right ] ] && [ center | [ top | bottom ] ] <length>?

 **Initial value:**

	50% 50%

 **Applies to:**

Any transformable element

 **Inherited:**

No

 **Percentages:**

Refer to the size of the bounding box (see explanation)

 **Computed value:**

A percentage, except for length values, which are converted to an absolute length

---


## Choosing a 3D Style

---

`transform-style`

 **Values:**

	flat | preserve-3d

 **Initial value:**

	flat

 **Applies to:**

Any transformable element

 **Inherited:**

No

 **Computed value:**

As specified

---


## Creating Perspective

two properties

### Defining a group perspective

---

`perspective`

 **Values:**

	none | <length>

 **Initial value:**

	none

 **Applies to:**

Any transformable element

 **Inherited:**

No

 **Computed value:**

The absolute length, or else `none`

---

### Moving the perspective’s origin

---

`perspective-origin`

 **Values:**

	  [ left | center | right | top | bottom | <percentage> | <length> ]
	|
	  [ left | center | right | <percentage> | <length> ]
	  [ top | center | bottom | <percentage> | <length> ] <length>?
	|
	  [ center | [ left | right ] ] && [ center | [ top | bottom ] ]

 **Initial value:**

	50% 50%

 **Applies to:**

Any transformable element

 **Inherited:**

No

 **Percentages:**

Refer to the size of the bounding box (see explanation)

 **Computed value:**

A percentage, except for length values, which are converted to an absolute length

---

## Dealing With Backfaces


---

`backface-visibility`

 **Values:**

	visible | hidden

 **Initial value:**

	visible

 **Applies to:**

Any transformable element

 **Inherited:**

No

 **Computed value:**

As specified

---
