# X

# Transforms

Ever since the inception of CSS, elements have been rectangular and very firmly oriented on the horizontal and vertical axes.  A number of tricks arose to make elements look like they were tilted and so on, but underneath it all was a very rigid grid.  In the late 2000s, there grew an interest in being able to break the shackles of that grid and transform objects in interesting ways—and not just in two dimensions.

If you’ve ever positioned an object, whether relatively or absolutely, then you’ve already transformed an object.  For that matter, any time you used floats or negative-margin tricks (or both), you transformed an object.  All of those are examples of translation, or the movement of an element from where it would normally appear to some other place.  With CSS Transforms, you have a new way to translate elements, and a whole lot more besides.  Whether it’s as simple as rotating some photographs a bit to make them appear more natural, or creating interfaces where information can be revealed by flipping over elements, or just doing interesting perspective tricks with sidebars, CSS Transforms can—if you’ll pardon the obvious expression—transform the way you design

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

You can use a degree angle to define skews, which will be covered later, but most authors will use degrees for rotational purposes.

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

First off, let's clear up the matter of the *bounding box*. For any element being affected by CSS, this is the border box; that is, the outermost edge of the element's border. That means that any outlines and margins are ignored for the purposes of calculating the bounding box. Special note: if a table-display element is being transformed, its bounding box is the table wrapper box, which encloses the table box and any association caption box.

If you're transforming an SVG element with CSS, then its bounding box is its SVG-defined _object bounding box_. Simple enough!

Now, the value entry *<transform-list>* requires some explanation. This placeholder refers to a list of one or more transform functions, one after the other, in space-separated format. It looks like this, with the result shown in Figure 6:

	#example {transform: rotate(30deg) skewX(-25deg) scaleY(2);}

> [[ Figure 6. A transformed div element. ]]

The functions are processed one at a time, starting with the first (leftmost) and proceeding to the last (rightmost). This first-to-last processing order is important, because changing the order can lead to drastically different results. Consider the following two rules, which have the results shown in Figure 7.

	img#one {transform: translateX(200px) rotate(45deg);}
	img#two {transform: rotate(45deg) translateX(200px);}

> [[ Figure 7. Different transform lists, different results. ]]

In the first instance, an image is translated (moved) 200 pixels along its X axis, and then rotated 45 degrees. In the second instance, an image is rotated 45 degrees and then moved 200 pixels along its X axis—that's the X axis of the transformed element, _not_ of the parent element, page, or viewport. In other words, when an element is rotated, its X axis (along with its other axes) rotates along with it. All element transforms are conducted with respect to the element's own frame of reference.

Compare this to a situation where an element is translated and then scaled, or vice versa; it doesn't matter which is which, because the end result is the same.

	img#one {transform: translateX(100px) scale(1.2);}
	img#two {transform: scale(1.2) translateX(100px);}

The situations where the order doesn't matter are far outnumbered by the situations where it does, so in general it's a good idea to just assume the order always matters, even when it technically doesn't.

Note that when you bave a series of transform functions, all of them must be properly formatted; that is, valid. If even one function is invalid, it renders the entire value invalid. Consider:

	img#one {transform: translateX(100px) scale(1.2) rotate(22);}

Because the value for `rotate()` is invalid—rotational values must have a unit—the entire value is dropped. The image in question will simply sit there in its initial untransformed state, neither translated nor scaled, let alone rotated.

It's also the case that transforms are not usually cumulative. That is to say, if you apply a transform to an element and then later want to add a transformation, you need to restate the original transform. Consider the following scenarios, illustrated in Figure 8:

	#ex01 {transform: rotate(30deg) skewX(-25deg);}
	#ex01 {transform: scaleY(2);}
	#ex02 {transform: rotate(30deg) skewX(-25deg);}
	#ex02 {transform: rotate(30deg) skewX(-25deg) scaleY(2);}

> [[ Figure 8. Overwriting or modifying transforms. ]]

In the first case, the second rule completely replaces the first, meaning that the element is only scaled along the Y axis. This actually makes some sense; it's the same as if you declare a font size and then elsewhere declare a different font size for the same element. You don't get a cumulative font size that way. You just get one size or the other. In the second example, the entirety of the first set of transforms was included in the second set, so they all got applied along with the `scaleY()` function.

There is an exception to this, which is that animated transforms, whether using transitions or actual animations, _are_ additive. That way, you can take an element that's transformed and then animate one of its transform functions without overwriting the others. For example, assume you had:

	img#one {transform: translateX(100px) scale(1.2);}

If you then animate the element's rotation angle, it will rotate from its translated, scaled state to the new angle, and its translation and scale will remain in place.

What makes this interesting is that even if you don't explicitly specify a transition or animation, you can still create additive transforms via the user-interaction pseudo-classes, such as `:hover`. That's because things like hover effects are types of transitions—they're just not invoked using the transition properties. Thus, you could declare:

	img#one {transform: translateX(100px) scale(1.2);}
	img#one:hover {transform: rotate(-45deg);}

This would rotate the translated, scaled image 45 degrees to its left on hover. The rotation would take place over zero seconds, because no transition interval was declared, but it's still an implicit transition. Thus, any state change can be thought of as a transition, and thus any transforms that are applied as a result of those state changes are additive with previous transforms.

> As of mid-2014, `transform` and its associated properties still had to be vendor-prefixed in WebKit and Blink browsers like Safari and Chrome. No prefixes were needed in other major user agents.

## The transform functions

There are, as of this writing, 21 different transform functions, employing a number of different value patterns to get their jobs done. Table 1 provides a list of all the available transform functions, minus their value patterns.

> Table 1. Transform functions.

<table>
<tr style="vertical-align: top;">
<td>
<code>translate()</code><br>
<code>translate3d()</code><br>
<code>translateX()</code><br>
<code>translateY()</code><br>
<code>translateZ()</code>
</td><td>
<code>scale()</code><br>
<code>scale3d()</code><br>
<code>scaleX()</code><br>
<code>scaleY()</code><br>
<code>scaleZ()</code>
</td><td>
<code>rotate()</code><br>
<code>rotate3d()</code><br>
<code>rotateX()</code><br>
<code>rotateY()</code><br>
<code>rotateZ()</code>
</td><td>
<code>skew()</code><br>
<code>skewX()</code><br>
<code>skewY()</code><br>
<code>matrix()</code><br>
<code>matrix3d()</code><br>
<code>perspective()</code>
</td>
</tr>
</table>


As previously stated, the most common value pattern for `transform` is a space-separated list of one or more functions, processed from first (leftmost) to last (rightmost), and all of the functions must have valid values. If any one of the functions is invalid, it will invalidate the entire value of `transform`, thus preventing any transformation at all.

### Translation functions

A translation transform is just a move along one or more axes. For example, `translateX()` moves an element along its own X axis, `translateY()` moves it along its Y axis, and `translateZ()` along its Z axis.

---
**Functions**

	translateX(), translateY()

**Permitted values**

	<length> | <percentage>
---

These are usually referred to as the "2D" translation functions, since they can slide an element up and down, or side to side, but not forward or backward along the Z axis. Each of these functions accepts a single distance value, expressed as either a length or a percentage.

If the value is a length, then the effect is about what you'd expect. Translate an element 200 pixels along the X axis with `translateX(200px)`, and it will move 200 pixels to its right. Change that to `translateX(-200px)`, and it will move 200 pixels to its left. For `translateY()`, positive values move the element downward; negative values move it upward, both with respect to the element itself. Thus, if you flip the element upside-down by rotation, positive `translateY()` values will actually move the element downward on the page.

If the value is a percentage, then the distance is calculated as a percentage of the element's own size. Thus, `translateX(50%)` will move an element 300 pixels wide and 200 pixels tall to its right by 150 pixels, and `translateY(-10%)` will move that same element upward (with respect to itself) by 20 pixels.

---
**Function**

	translate()

**Permitted values**

	[ <length> | <percentage> ] [, <length> | <percentage>]?
---

If you want to translate an element along both the X and Y axes at the same time, then `translate()` makes that simple. Just supply the X value first and the Y value second, and it will act the same as if you combined `translateX() translateY()`. If you omit the Y value, then it's assumed to be zero. Thus, `translate(2em)` is treated as if it were `translate(2em,0)`, which is also the same as `translateX(2em)`. See Figure 9 for some examples of 2D translation.

> [[ Figure 9. Translating in two dimensions. ]]

According to the latest version of the specification, both the 2D translation functions can be given a unitless number. In that case, the number is treated as being expressed in terms of a "user unit,"which is treated the same as a pixel unless otherwise defined. The CSS specification does not explain how a user unit is otherwise defined: the SVG specification does, albeit briefly. In the field, no browser tested as of this writing supported unitless numbers of translation values, so the capability is academic at best.

---
**Function**

	translateZ()

**Permitted value**

	<length>
---


This function translates elements along the Z axis, thus moving them into the third dimension. Unlike the 2D translation functions, `translateZ()` only accepts length values. Percentage values are _not_ permitted for `translateZ()`, or indeed for any Z-axis value.

---
**Functions**

	translate3d()

**Permitted values**

	[ <length> | <percentage> ], [ <length> | <percentage>], [ <length> ]
---

Much like `translate()` does for X and Y translations, `translate3d()` is a shorthand function that incorporates the X, Y, and Z translation values into a single function. This is obviously handy if you want to move an element over, up, and forward in one fell swoop. See Figure 10 for some illustrations of 3D translations.

> [[ Figure 10. Translating in three dimensions. ]]

Unlike `translate()`, there is no fallback for situations where `translate3d()` does not contain three values. Thus, `translate3d(1em,-50px)` should be treated as invalid by user agents instead of being assumed to be `translate3d(2em,-50px,0)`.

### Scale functions

A scale transform makes an element larger or smaller, depending on what value you use. These values are unitless real numbers, and are always positive. On the 2D plane, you can scale along the X and Y axes individually, or scale them together.

---
**Functions**

	scaleX(), scaleY(), scaleZ()

**Permitted value**

	<number>
---


The number value supplied to a scale function is a multiplier; thus, `scaleX(2)` will make an element twice as wide as it was before the transformation, whereas `scaleY(0.5)` will make it half as tall. Given this, you might expect that percentage values are permissible as scaling values, but they aren't.

---
**Function**

	scale()

**Permitted value**

	<number> [, <number>]?
---

If you want to scale along both axes simultaneously, use `scale()`. The X value is always first and the Y always second, so `scale(2,0.5)` will make the element twice as wide and half as tall as it was before being transformed. If you only supply one number, it is used as the scaling value for both axes; thus, scale(2) will make the element twice as wide _and _twice as tall. This is in contrast to `translate()`, where an omitted second value is always set to zero. `scale(1)` will scale an element to be exactly the same size it was before you scaled it, as will scale(1,1). Just in case you were dying to do that.

Of course, if you can scale in two dimensions, you can also scale in three. CSS offers `scaleZ()` for scaling just along the Z axis, and `scale3d()` for scaling along all three axes at once.

---
**Function**

	scale3d()

**Permitted value**

	<number>, <number>, <number>
---

Similar to `translate3d()`, `scale3d()` requires all three numbers to be valid. If you fail to do this, then the malformed `scale3d() `will invalidate the entire transform value to which is belongs. See Figure 11 for some examples of element scaling.

> [[ Figure 11. Scaled elements. ]]


### Rotation functions

A rotation function causes an element to be rotated around an axis, or around an arbitrary vector in 3D space.  There are four simple rotation functions, and one less simple function meant specifically for 3D.

---
**Functions**

	rotate(), rotateX(), rotateY(), rotateZ()

**Permitted values**

	<angle>
---


All four basic rotation functions accept just one value, a degree.  This can be expressed using any of the valid degree units (`deg`, `grad`, `rad`, and `turn`) and a number, either positive or negative.

If a value’s number runs outside the usual range for the given unit, it will be normalized to fit into the accepted range.  In other words, a value of `437deg` will be tilted the same as if it were `77deg`—or, for that matter, `-283deg`.  Note, however, that these are only exactly equivalent if you don’t animate the rotation in some fashion.

That is to say, animating a rotation of `1100deg` will spin the element around several times before coming to rest at a tilt of -20 degrees (or 340 degrees, if you like).  By contrast, animating a rotation of `-20deg` will tilt the element a bit to the left, with no spinning; and of course animating a rotation of `340deg` will animate an almost-full spin to the right.  All three animations come to the same end state, but the process of getting there is very different in each case.

The function `rotate()` is a straight 2D rotation, and the one you’re most likely to use.  It is equivalent to `rotateZ()` because it rotates the element around the Z axis (the one that shoots straight out of your display and through your eyeballs).  In a like manner, `rotateX()` causes rotation around the X axis, thus causing the element to tilt toward or away from you; and `rotateY()` rotates the element around its Y axis, as though it were a door.  These are all illustrated in Figure 12.

> [[ Figure 12. Rotations around the three axes. ]]

> WARNING: The examples in Figure 12 all present a fully 3D appearance.  This is only possible with certain values of the properties `transform-style` and `perspective`, described in a later section.  This will be true throughout this text in any situation where 3D-transformed elements appear to be fully three-dimensional.
---

---
**Function**

	rotate3d()

**Permitted value**

	<number>, <number>, <number>, <angle>
---

If you’re comfortable with vectors and want to rotate an element through 3D space, then `rotate3d()` is for you.  The first three numbers specify the X, Y, and Z components of a vector in 3D space, and the degree value determines the amount of rotation around the declared 3D vector.

To start with a simple example, the 3D equivalent to `rotate(45deg)` is `rotate3d(0,0,1,45deg)`.  That specifies a vector of zero magnitude on the X and Y axes, and a magnitude of 1 along the Z axis.  In other words, it describes the Z axis.  The element is thus rotated 45 degrees around that vector, as shown in Figure 13.  That figure also shows the appropriate `rotate3d()` values to rotate an element by 45 degrees around the X and Y axes.

> [[ Figure 13. Rotations around 3D vectors. ]]

A little more complicated is something like `rotate3d(-0.25,0.35,0.66667,45deg)`, where the described vector points off in to 3D space between the axes.  This has the result shown (and illustrated via schematic) in Figure 14.

> [[ Figure 14. Rotation around a 3D vector, and how that vector is determined. ]]

If you’re no comfortable with vectors, that’s okay; most people aren’t.  You’ll really only ever need to use them if you’re doing precise 3D calculations, at which point you’ll be getting very familiar with vectors whether you want to or not.

### Skew functions

When you skew an element, you slant it along one or both of the X and Y axes.  There is no Z-axis or other 3D skewing.

---
**Functions**

	skewX(), skewY()

**Permitted value**

	<angle>
---

In both cases, you supply an angle value, and the element is skewed to match that angle.  It’s much easier to show skewing rather than try to explain it in words, so Figure 15 shows a number of skew examples along the X and Y axes.

> [[ Figure 15. Skewing along the X and Y axes. ]]


---
**Function**

	skew()

**Permitted values**

	<angle> [, <angle> ]?
---


The `skew()` function is just a shorthand for `skewX()` and `skewY()`, accepting either one degree value or two comma-separated degree values.  If you have two values, the X skew angle is always first, and the Y skew angle comes second.  If you leave out a Y skew angle, then it’s treated as zero.  This means `skew(45deg)` is functionally equivalent to `skewX(45deg)`.  Figure 16 shows some double-skewed elements.

> [[ Figure 16. Skewed elements. ]]


### The perspective function

If you’re transforming an element in 3D space, you most likely want it to have some perspective.  Perspective gives the appearance of front-to-back depth, and you can vary the amount of perspective applied to an element.

Function

	perspective()

Permitted values

	<length>

It might seem a bit weird that you specify perspective as a distance.  After all, `perspective(200px)` seems a bit odd when you can’t really measure pixels along the Z axis.  And yet, here we are.  You supply a length, and the illusion of depth is constructed around that value.  Lower numbers create more extreme perspective, as though you were right up close to the element and viewing it through a fisheye lens.  Higher numbers create a gentler perspective, as though viewing the element through a zoom lens from far away.  _Really_ high perspective values create an isometric effect.

This makes a certain amount of sense.  If you visualize perspective as a pyramid, with its apex point at the perspective origin and its base the closest thing to you, then a shorter distance between apex and base will create a shallower pyramid, and thus a more extreme distortion.  This is illustrated in Figure 17.

> [[ Figure 17. Different perspective pyramids. ]]

In the documentation for Safari, Apple writes that perspective values below `300px` tend to be extremely distorted, values above `2000px` create “very mild” distortion, and values between `500px` and `1000px` create “moderate perspective.” [^1]  To illustrate this, Figure 18 shows a series of elements with the exact same rotation as displayed with varying perspective values.

[^1]: https://developer.apple.com/library/safari/documentation/InternetWeb/Conceptual/SafariVisualEffectsProgGuide/Using2Dand3DTransforms/Using2Dand3DTransforms.html

> [[ Figure 18. The effects of varying perspective values. ]]

Perspective values must always be positive, non-zero lengths.  Any other value will cause the `perspective()` function to be ignored.  Also note that its placement in the list of functions is very important.  If you look at the code for Figure 18, the `perspective()` function comes before the `rotateY()` function.  If you were to reverse the order, the rotation would happen before the perspective is applied, so all four examples in Figure 18 would look exactly the same.  So if you plan to apply a perspective value via the list of transform functions, make sure it comes first, or at the very least before any transforms that depend on it.  This serves as a particularly stark reminder that the order you write `transform` functions can be very important.

> !! Note that the function `perspective()` is very similar to the property `perspective`, which will be covered later, but they are applied in critically different ways.  Generally, you will want to use the `perspective` property instead of the `perspective()` function, but there may be exceptions.


### Matrix functions

If you’re a particular fan of advanced math, or stale jokes derived from Wachowski Brothers movies, then these functions will be your favorites.

---
**Function**

	matrix()

**Permitted values**

	 <number> [, <number> ]{5,5}
---

In the CSS Transforms specification, we find the trenchant description of `matrix()` as a function that “specifies a 2D transformation in the form of a transformation matrix of the six values *a*-*f*.”

First things first: a valid `matrix()` value is a list of six comma-separated numbers.  No more, no less.  The values can be positive or negative.  Second, the number describe the final transformed state of the element, combining all of the other transform types (rotation, skewing, etc.) into a very compact syntax.  Third, very few people actually use this syntax.

Here’s a brief rundown of how it works.  Say you have this function applied to an element:

	matrix(0.838671, 0.544639, -0.692519, 0.742636, 6.51212, 34.0381)

That’s the CSS syntax used to describe this transformation matrix:

	0.838671	-0.692519	0	6.51212
	0.544639	 0.742636	0	34.0381
	0			 0			1	0
	0			 0			0	1

Right.  So what does that do?  It has the result shown in Figure 19, which is exactly the same result as writing this:

	rotate(33deg) translate(24px,25px) skewX(-10deg)

> [[ Figure 19. A matrix-transformed element and its functional equivalent. ]]

What this comes down to is, if you’re familiar with or need to make use of matrix calculations, you can and should absolutely use them.  If not, you can chain much more human-readable transform functions together and get the element to the same end state.

Now, that was for plain old 2D transforms.  What if you want to use a matrix to transform through three dimensions?

---
**Function**

	matrix3d()

**Permitted values**

	<number> [, <number> ]{15,15}
---

Again, just for kicks, we’ll savor the definition of `matrix3d()` from the CSS Transforms specification: “specifies a 3D transformation as a 4x4 homogeneous matrix of 16 values in column-major order.”  This means the value of `matrix3d` _must_ be a list of 16 comma separated numbers, no more or less.  Those numbers are arranged in a 4x4 grid in column order, so the first column is the first set of four numbers in the value, the second column the second set of four numbers, the third column the third set, and so on.  Thus, you can take the following function:

	matrix3d(
		1, 0, 0, 0,
		-0.176327, 0.838671, 0.544639, -0.0027232,
		0, -0.544639, 0.838671, -0.00419335,
		24, 20.9668, 13.616, 0.93192)

…and write it out as this matrix:

	1			0			0			0
	-0.176327	0.838671	0.544639	-0.0027232
	0			-0.544639	0.838671	-0.00419335
	24			20.9668		13.616		0.93192

…both of which have an end state equivalent to:

	perspective(500px) rotate(33deg) translate(24px,25px) skewX(-10deg)

…as shown in Figure 20.

> [[ Figure 20. A matrix3d-transformed element and its functional equivalent. ]]

### A note on end-state equivalence

It’s important to keep in mind that only the end states of a `matrix()` function, and an equivalent chain of transform functions, can be considered identical.  This is for the same reason that was discussed in the section on rotation: because a rotation angle of `393deg` will end with the same visible rotation as an angle of `33deg`.  This matters if you are animating the transformation, since the former will cause the element to do a barrel roll in the animation, whereas the second will not.  The `matrix()` version of this end state won’t include the barrel roll, either.  Instead, it will always use the shortest possible rotation to reach the end state.

To illustrate what this means, consider the following, a transform chain and its `matrix()` equivalent:

	rotate(200deg) translate(24px,25px) skewX(-10deg)
	matrix(-0.939693, -0.34202, 0.507713, -0.879385, -14.0021, -31.7008)

Note the rotation of 200 degrees.  We naturally interpret this to mean a clockwise rotation of 200 degrees, which it does.  If these two transforms are animated, however, they will have act differently: the chained-functions version will indeed rotate 200 degrees clockwise, whereas the `matrix()` version will rotate 160 degrees anti-clockwise.  Both will end up in the same place, but will get there in different ways.

There are similar differences that arise even when you might think they wouldn’t.  Once again, this is because a `matrix()` transformation will always take the shortest possible route to the end state, whereas a transform chain might not.  (In fact, it probably doesn’t.)  Consider these apparently equivalent transforms:

	rotate(160deg) translate(24px,25px) rotate(-30deg) translate(-100px)
	matrix(-0.642788, 0.766044, -0.766044, -0.642788, 33.1756, -91.8883)

As ever, they end up in the same place.  When animated, though, the elements will take different paths to reach that end state.  They might not be obviously different at first glance, but the difference is still there.

Of course, none of this matters if you aren’t animating the transformation, but it’s an important distinction to make nevertheless, because you never know when you’ll decide to start animating things.  (Hopefully after reading the companion text on animations!)


# More Transform Properties

In addition to the base `transform` property, there are a few related properties that help to define things like the origin point of a transform, the perspective used for a “scene,” and more.

## Moving the Origin

So far, all of the transforms we’ve seen have shared one thing in common: the precise center of the element was used as the *transform origin*.  For example, when rotating the element, it rotated around its center, instead of, say, a corner.  This is the default behavior, but with the property `transform-origin`, you can change it.

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

With `transform-origin`, you supply two keywords to define the point around which transforms should be made; first the horizontal, then the vertical.  (If you’re aware of the `background-position` property, the syntax for `transform-origin` should look very familiar.)  You can use plain-English keywords like `top` and `right`, percentages, lengths, or a combination of different keyword types.

Length values are taken as a distance from the top left corner of the element.  Thus, `transform-origin: 5em 22px` will place the transform origin 5em to the right of the left side of the element, and 22 pixels down from the top of the element.

Percentages are calculated with respect to the corresponding axis and size of the element, as offsets from the element’s top left corner.  For example, `transform-origin: 67% 40%` will place the transform origin 67 percent of the width to the right of the element’s left side, and 40 percent of the element’s height down from the element’s top side.  Figure 21 illustrates a few origin calculations.

> [[ Figure 21. Various origin calculations. ]]

All right, so if you change the origin, what happens?  The easiest way to visualize this is with rotations.  Suppose you rotate an element 45 degrees to the right.  Its final placement will depend  on its origin.  Figure 22 illustrates the the effects of several different transform origins.

> [[ Figure 22. The rotational effects of using various transform origins. ]]

The origin matters for other transform types, such as skews and scales.  Scaling an element with its origin in the center will pull in all sides equally, whereas scaling an element with a bottom-right origin will cause it to shrink toward that corner.  Similarly, skewing an element with respect to its center will result in the same shape as if it’s skewed with respect to the top right corner, but the placement of the shape will be different.  Some examples are shown in Figure 23.

> [[ Figure 23. The skew effects of using various transform origins. ]]

The one transform type that isn’t really affected by changing the transform origin is translation.  If you push an element around with `translate()`, or its cousins like `translateX()` and `translateY()`, it’s going to end up in the same place regardless of where the transform origin is located.  If that’s all the transforming you plan to do, then setting the transform origin is irrelevant.  If you ever do anything besides translating, though, the origin will matter.  Use it wisely.

## Choosing a 3D Style

If you’re setting elements to be transformed through three dimensions—using, say, `translate3d()` or `rotateY()`—you probably expect that the elements will be presented as though they’re in a 3D space.  And yet, this is not the default behavior.  By default, everything looks flat no matter what you do.  Fortunately, this can be overridden with the `transform-style` property.

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

Suppose you have an element you want to move “closer to” your eye, and then tilt away a bit, with a moderate amount of perspective.  Something like this rule, as applied to the following HTML:

	div#inner {transform: perspective(750px) translateZ(60px) rotateX(45deg);}
	
	<div id="outer">
	outer
	<div id="inner">inner</div>
	</div>

So you do that, and get the result shown in Figure 24; more or less what you might have expected.

> [[ Figure 24. A 3D-transformed inner `div`. ]]

But then you decide to rotate the outer `div` to one side, and suddenly nothing makes sense any more.  The inner `div` isn’t where you envisioned it.  In fact, it just looks like a picture pasted to the front of the outer `div`.

Well, that’s exactly what it is, because the default value of `transform-style` is `flat`.  What happened was, the inner `div` got drawn in its moved-forward-tilted-back state, and that was applied to the front of the outer `div` like it was an image.  So, when you rotated the outer `div`, the flat picture rotated right along with it, as shown in Figure 25.

	div#outer {transform: perspective(750px) rotateY(60deg) rotateX(-20deg);}

> [[ Figure 25. The effects of a `flat` transform style. ]]

Change the value to `preserve-3d`, however, and things suddenly change.  The inner `div` will be drawn as a full 3D object with respect to its parent outer `div`, floating in space nearby, and _not_ as a picture pasted on the front of the outer `div`.  You can see the results of this change in Figure 26.

	div#outer {transform: perspective(750px) rotateY(60deg) rotateX(-20deg);
		transform-style: preserve-3d;}
	div#inner {transform: perspective(750px) translateZ(60px) rotateX(45deg);}

> [[ Figure 26. The effects of a 3D-preserved transform style. ]]

One important aspect of `transform-style` is that it can be overridden by other properties.  The reason is that some values of these other properties require a flattened presentation of an element and its children in order to work at all.  In such cases, the value of `transform-style` is forced to be `flat` regardless of what you may have declared.

So, in order to avoid this overriding behavior, make sure the following properties are set to the listed values:

*	`overflow: visible`
*	`filter: none`
*	`clip: auto`
*	`clip-path: none`
*	`mask-image: none`
*	`mask-border-source: none`
*	`mix-blend-mode: normal`

Those are all the default values for those properties, so as long as you don’t try to change any of them for your preserved-3D elements, you’re fine!  But if you find that editing some CSS suddenly flattens out your lovely 3D transforms, one of these properties might be the culprit.

One more note: in addition to the above, the value of the property `isolation` must be, or be computed to be, `isolate`.  (`isolation` is a compositing property, in case you were wondering.)

## Changing Perspective

There are actually two properties that are used to define how perspective is handled: one to define the perspective distance, as with the `perspective()` function discussed in an earlier section; and another to define the perspective’s origin point.

### Defining a group perspective

First, let’s consider the property `perspective`, which accepts a length that defines the depth of the perspective pyramid.  At first glance, it looks just like the `perspective()` function, but there are some very critical differences.

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

As a quick example, if you want to create a very deep perspective, one mimicking the results you’d get from a zoom lens, you might declare something like `perspective: 2500px`.  For a shallow depth, one that mimics a closeup fisheye lens effect, perhaps `perspective: 100px`.

So how does this differ from the `perspective()` function?  When you use `perspective()`, you’re defining the perspective effect for the element that is given that function.  So if you say `transform: rotateY(-50grad) perspective(800px);`, you’re applying that perspective to each element that has the rule applied.

With the `perspective` property, on the other hand, you’re creating a perspective depth that is applied to all the child elements of the element that received the property.  Confused yet?  Don’t be.  Here’s a simple illustration of the difference, as shown in Figure 27.

	div {transform-style: preserve-3d; border: 1px solid gray; width: 660px;}
	img {margin: 10px;}
	#one {perspective: none;}
	#one img {transform: perspective(800px) rotateX(-50grad);}
	#two {perspective: 800px;}
	#two img {transform: rotateX(-50grad);}

	<div><img src="rsq.gif"><img src="rsq.gif"><img src="rsq.gif"></div>
	<div id="one"><img src="rsq.gif"><img src="rsq.gif"><img src="rsq.gif"></div>
	<div id="two"><img src="rsq.gif"><img src="rsq.gif"><img src="rsq.gif"></div>

> [[ Figure 27. Shared perspective versus individual perspectives. ]]

In Figure 27, we see first a line of images that haven’t been transformed.  In the second line, each image has been rotated 50 gradians (equivalent to 45 degrees) toward us, but each one within its own individual perspective.

In the third line of images, none of them has an individual perspective.  Instead, they are all drawn within the perspective defined by the `perspective: 800px;` that’s been set on the `div` that contains them.  Since they all operate within a shared perspective, they look “correct;” that is, like we would expect if we had three physical pictures mounted on a clear sheet of glass and rotated toward us around the center horizontal axis of that glass.

> Note that presence of `transform-style: preserve-3d` makes this effect possible, as discussed in the previous section.

This is the critical difference between `perspective`, the property; and `perspective()`, the function.  The former creates a 3D space shared by all its children.  The latter affects only the element to which it’s applied.

In most cases, you’re going to use the `perspective` property.  In fact, container `div`s (or other elements) are a very common feature of 3D transforms, the way they used to be for page layout.  In the previous example, the `<div id="two">` was there solely to serve as a perspective container, so to speak.  On the other hand, we couldn’t have done what we did without it.


### Moving the perspective’s origin

When transforming elements in three dimensions—assuming you’ve allowed them to appear three-dimensional, that is—there will be a perspective used.  (See `transform-style` and `perspective`, respectively, in previous sections.)  That perspective will have an origin, which is also known as the _vanishing point_, and you can change where it’s located with the property `perspective-origin`.

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

As you’ve no doubt spotted, `perspective-origin` and `transform-origin` (see previously) have the same value syntax.  While the way the values are expressed is identical, the effects they have are very different.  With `transform-origin`, you define the point around which transforms happen.  With `perspective-origin`, you define the point on which sight lines converge.

As with most 3D transform properties, this is more easily demonstrated than described.  Consider the following CSS and markup, illustrated in Figure 28.

	#container {perspective: 850px; perspective-origin: 50% 0%;}
	#ruler {height: 50px; background: #DED url(tick.gif) repeat-x;
		transform: rotateX(60deg);
		transform-origin: 50% 100%;}

	<div id="container">
		<div id="ruler"></div>
	</div>

> [[ Figure 28. A basic “ruler.” ]]

What we have is a repeated background image of tick-marks on a ruler, with the `div` that contains them tiled away from us by 60 degrees.  All the lines point at a common vanishing point, the top center of the container `div` (because of the `50% 0%` value for `perspective-origin`).

No consider that same setup with various perspective origins, as shown in Figure 29.

> [[ Figure 29. A basic “ruler” with different perspective origins. ]]

As you can see, moving the perspective origin changes the rendering of the 3D-transformed element.

Note that these only had an effect because a we supplied a value four `perspective`.  If the value of `perspective` is ever `none`, then any value given for `perspective-origin` will be ignored.  That makes sense, since you can’t have a perspective origin when there’s no perspective at all!

## Dealing With Backfaces

Something you probably never really thought about, when laying out elements, was: what would it look like if I could see the back side of the element?  But now that 3D transforms are a possibility, there may well come the day when you _do_ see the back of an element.  You might even mean to do so intentionally.  What happens at that moment is determined by the property `backface-visibility`.

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

Unlike many of the other properties and functions we’ve already talked about, this one is as straightforward as straightforward can be.  All it does is determine whether the back side of an element can be seen, or not.  It really is just that simple.

So let’s say you flip over two elements, one with `backface-visibility` set to the default value of `visible` and the other set to `hidden`.  You get the result shown in Figure 30.

	span {border: 1px solid red; display: inline-block;}
	img {vertical-align: bottom;}
	img.flip {transform: rotateX(180deg); display: inline-block;}
	img#show {backface-visibility: visible;}
	img#hide {backface-visibility: hidden;}

	<span><img src="salmon.gif"></span>
	<span><img src="salmon.gif" class="flip" id="show"></span>
	<span><img src="salmon.gif" class="flip" id="hide"></span>

> [[ Figure 30. Visible and hidden backfaces. ]]

As you can see, the first image is unchanged.  The second is flipped over around its X axis, so we see it from the back.  The third has also been flipped, but we can’t see its back because it’s hidden.

This property can come in handy in a number of situations.  The simplest is a case where you have two elements that represent the two sides of a UI element that flips over; say, a search area with preference settings on its back, or a photo with some information on the back.  Let’s take the latter case.  The CSS and markup might look something like this:

	section {position: relative;}
	img, div {position: absolute; top: 0; left: 0; backface-visibility: hidden;}
	div {transform: rotateY(180deg);}
	section:hover {transform: rotateY(180deg); transform-style: preserve-3d;}
	
	<section>
		<img src="photo.jpg" alt="">
		<div class="info">(…info goes here…)</div>
	</section>

Okay, so that example shows that using `backface-visibility` isn’t _quite_ as simple as it first appears.  It’s not that the property itself is complicated, but if you forget to set `transform-style` to `preserve-3d`, then it won’t work as intended.

There’s a variant on that example that uses the same markup, but slightly different CSS to show the image’s back face when it’s flipped over.  This is probably more what was intended, since it makes it look like the information is literally written on the back of the image.  It leads to the end result shown in Figure 31.

	section {position: relative;}
	img, div {position: absolute; top: 0; left: 0;}
	div {transform: rotateY(180deg); backface-visibility: hidden;
		background: rgba(255,255,255,0.85);}
	section:hover {transform: rotateY(180deg); transform-style: preserve-3d;}

> [[ Figure 31. Information on the back. ]]

The only thing we had to do to make that happen was to just shift the `backface-visibilty: hidden` to the `div` instead of applying it to both the `ing` and the `div`.  Thus, the `div`’s backface is hidden when it’s flipped over, but the image’s is not.

# Summary

With the ability to transform elements in two- and three-dimensional space, CSS Transforms provide a great deal of power to designers who are looking for new ways to present information.  From creating interesting combinations of 2D transforms to creating a fully 3D-acting interface, transforms open up a great deal of new territory in the design space.   There are some interesting dependencies between properties, which is something that not every CSS author will find natural at first, but they become second nature with just a bit of practice.