# X

# Transforms

introduction

# Coordinate Systems

Before embarking on this journey, let's take a moment to orient ourselves. There are two types of coordinate systems that are used in transforms, and it's a good idea to be familiar with both. (If you're already well familiar with Cartesian and spherical coordinate systems, particularly as used in computing, feel free to skip to the next section.)

The first is the Cartesian coordinate system, or what's often called the X/Y/Z coordinate system. This system is a way of describing the position of a point in space using two numbers (for two-dimensional placement) or three numbers (for three-dimensional placement). In CSS, the system uses three axes: the X or horizontal axis, the Y or vertical axis, and the Z or depth axis. This is illustrated in Figure 1.

[[]]  
Figure 1. The three Cartesian axes used in CSS transforms.

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

[[]]

Figure 2. Elemental frames of reference.

Speaking of rotations, the other coordinate system used in CSS transforms is a spherical system, which describes angles in 3D space. It's illustrated in Figure 3.

[[]]

Figure 3. The spherical coordinate system used in CSS transforms.

For purposes of 2D transforms, you only have to worry about a single 360-degree polar system: the one that sits on the plane described by the X and Y axes. This actually describes rotations around the Z axis, since that's the axis that runs through the center of the circle like the axle of a wheel. Similarly, rotations around the X axis tilt the element toward or away from you, and rotations around the Y axis turn the element from side to side. These are illustrated in Figure 4.

[[]]

Figure 4. Rotations around the three axes.

But back to 2D rotations. Suppose you wanted to rotate an element 45 degrees clockwise in the plane of the display (i.e., around the Z axis). The transform value is:

	rotateZ(45deg)

Change that to `–45deg`, and the element will rotate counter-clockwise (anti-clockwise for our international friends) around the Z axis. In other words, it will rotate in the X/Y plane, as illustrated in Figure 5.

[[]]

Figure 5. Rotations in the XY plane.

You can use degree to define skews, which will be covered later, but most authors will use degrees for rotational purposes.

All right—now that we have our bearings, let's get started with transforms!

# Transforming

There's really only one property that applies transforms, along with a few ancillary properties that affect exactly how the transforms are applied. We'll start with the Big Cheese.

transform

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

First off, let's clear up the matter of the bounding box. For any element being affected by CSS, this is the border box; that is, the outermost edge of the element's border. That means that any outlines and margins are ignored for the purposes of calculating the bounding box. Special note: if a table-display element is being transformed, its bounding box is the table wrapper box, which encloses the table box and any association caption box.

If you're transforming an SVG element with CSS, then its bounding box is its SVG-defined _object bounding box_. Simple enough!

Now, the value entry *<transform-list>* requires some explanation. This placeholder refers to a list of one or more transform functions, one after the other, in space-separated format. It looks like this, with the result shown in Figure 6:

	#example {transform: rotate(30deg) skewX(-25deg) scaleY(2);}

[[]]

Figure 6. A transformed div element.

The functions are processed one at a time, starting with the first (leftmost) and proceeding to the last (rightmost). This first-to-last processing order is important, because changing the order can lead to drastically different results. Consider the following two rules, which have the results shown in Figure 7.

	img#one {transform: translateX(200px) rotate(45deg);}

	img#two {transform: rotate(45deg) translateX(200px);}

[[]]

Figure 7. Different transform lists, different results.

In the first instance, an image is translated (moved) 200 pixels along its X axis, and then rotated 45 degrees. In the second instance, an image is rotated 45 degrees and then moved 200 pixels along its X axis—that's the X axis of the transformed element, _not_ of the parent element, page, or viewport. In other words, when an element is rotated, its X axis (along with its other axes) rotates along with it. All element transforms are conducted with respect to the element's own frame of reference.

Compare this to a situation where an element is translated and then scaled, or vice versa; it doesn't matter which is which, because the end result is the same.

	img#one {transform: translateX(100px) scale(1.2);}

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

[[]]

Figure XX. Overwriting or modifying transforms.

In the first case, the second rule completely replaces the first, meaning that the element is only scaled. This actually makes some sense; it's the same as if you declare a font size and then elsewhere declare a different font size for the same element. You don't get a cumulative font size that way. You just get one size or the other. In the second example, the entirety of the first set of transforms was included in the second set, so they all got applied along with the scaleY() function.

There is an exception to this, which is that animated transforms, whether using transitions or actual animations, _are_ additive. That way, you can take an element that's transformed and then animate one of its transform functions without overwriting the others. For example, assume you had:

	img#one {transform: translateX(100px) scale(1.2);}

If you then animate the element's rotation angle, it will rotate from its translated, scaled state to the new angle, and its translation and scale will remain in place.

What makes this interesting is that even if you don't explicitly specify a transition or animation, you can still create additive transforms via the user-interaction psuedo-classes, such as `:hover`. That's because things like hover effects are types of transitions—they're just not invoked using the transition properties. Thus, you could declare:

	img#one {transform: translateX(100px) scale(1.2);}

	img#one:hover {transform: rotate(-45deg);}

This would rotate the translated, scaled image 45 degrees to its left on hover. The rotation would take place over zero seconds, because no transition interval was declared, but it's still an implicit transition. Thus, any state change can be thought of as a transition, and thus any transforms that are applied as a result of those state changes are additive with previous transforms.

> As of mid-2014, transform still had to be vendor-prefixed in WebKit and Blink browsers like Safari and Chrome. No prefixes were needed in other major user agents.

## Transform functions

There are, as of this writing, 21 different transform functions, employing a number of different value patterns to get their jobs done. Table 1 provides a list of all the available transform functions, minus their value patterns.

Table . Transform functions.


| `translate()`<br>`translate3d()`<br>`translateX()`<br>` translateY()`<br>` translateZ()` | `scale()`<br>` scale3d()`<br>` scaleX()`<br>` scaleY()`<br>`scaleZ()` | `rotate()`<br>` rotate3d()`<br>` rotateX()`<br>` rotateY()`<br>` rotateZ()` | `skew()`<br>` skewX() `<br>`skewY() `<br>`matrix()`<br>` matrix3d()`<br>` perspective()` |


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

[[]]

Figure XX. Translating in two dimensions.

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

[[]]

Figure XX. Translating in three dimensions.

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

[[]]

Figure XX. Scaled elements.

### Rotation functions

Functions

	rotate(), rotateX(), rotateY(), rotateZ()

Permitted values

	<angle>



Function

	rotate3d()

Permitted value

	<number>, <number>, <number>, <angle>



### Skew functions

Functions	skewX(), skewY()Permitted value	<angle>Function	skew()Permitted values	<angle> [, <angle> ]?



### Matrix functions

much math here (yikes)

### The perspective function



Function

	perspective()

Permitted values

	<length>
