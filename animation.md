X
================

# Animation

CSS Transitions enable simple animations, but the start and end states of the animation are controlled by the existing property values, and provide us with little to no control over how the animation progresses.

Animations are similar to transitions in that they change the value of CSS properties over time. But unlike transitions, where we can only use timing functions to control how the animation goes from the original value to the final value, CSS keyframe animations enable us to granularly control what happens throughout the animation. With transitions, you can only go from the existing value to a new value: with CSS animations, the property values set on the animated element don't necessarily have to be part of the animation progression.With transitions, going from black to white will only display varying shades of grey. With animation, you can stick with only grays, or you can animate thru various colors: use as many keyframes as you want to create the desired effect.

While transitions trigger implicit property values changes, animations are explicitly executed when the animation keyframe properties are applied.

CSS animation enables us to animate the values of CSS properties over time, using keyframes. Similar to transitions, animation provides us with greater control over the duration, number of repeats, the repeating behavior, and what happens before the first animation commences and after the last animation iteration concludes. CSS animation properties allow us to control the timing, and even pause an animation mid-stream.

The first step in creating an animation is creating a keyframe animation that defines which properties will be animated and how with an @keyframes at-rule. The second step is to apply the keyframe animation you created to one or more elements in your document or application, defining how that animation will progress with the various animation properties

## Keyframes

To animate, within a selector's CSS we provide, at minimum, the name and duration of a keyframe animation. We define this CSS animation separately using the @keyframes at-rule.

To define our CSS animation, we need to declare its keyframes using the @keyframes rule. We give our keyframe animation a name which we use within a CSS selector's code block to attach this particular animation to the elements and pseudo elements selected by that particular code block.

The @keyframes at-rule includes the animation identifier and one or more keyframe blocks. Each keyframe block includes one or more keyframe selectors and a declaration block of zero or more property / value pairs.

    @keyframes animation_identifier {
      keyframe_selector {
        properties: values;
      }
    }

The animation identfier is the name you give your animation for future reference. The keyframe block includes the keyframe selector is the keyframe time position along the duration of the animation in either percentages or the keyterms `from` or `two`, and the declaration block, which includes the series of zero or more property value pairs enclosed in curly braces.

## Setting up your keyframe animation

We create a `@keyframes` at-rule, with an animation name, and a series of keyframe selectors with blocks of CSS in which you declare the properties we want to animate at that percentage of the way thru the animation. The keyframe we declare don't in themselves animate anything. Rather, we must attach the keyframe animations we created via the animation-name property, whose value is the name we provided within our at-rule.

Start with the at-rule declaration, followed by a name you create, and brackets:

	@keyframes the_name_you want {
    ...
	}

The name, which you create, is an identifier, not a string, which we later reference by the `animation-name` property. Identifiers have specific rules. First, they can't be quoted. You can use any characters [a-zA-Z0-9], the hyphen (-), underscore (_), and and any ISO 10646 character U+00A0 and higher.
ISO 10646 is the universal character set. This means you can use any character in the unicode standard that matches the regular expression [-_a-zA-Z0-9\u00A0-\u10FFFF].

There are some limitations on the name. As mentioned above, do not quote your animation name. The name can't start with a digit [0-9], two hyphens, though one hyphen is fine as long as it is not followed by a digit, unless it's escaped with a back slash. Make sure to escape them any escape characters. For example, Q&A must be written as Q\&A\!. âœŽ can be left as âœŽ, but if you are going to use any keyboard characters that aren't letters or digits, like !, @, #, $, %, ^, &, *, (, ), +, =, ~, `, ,, ., ', ", ;, :, [, ], {, }, |, \ and /, escape it with a back slash.

Also, don't use any of the keyterms covered in this chapter as the name for your animation. For example, possible values for the animation properties include `none`, `paused`, `running`, `infinite`, `backwards`, and `forwards`, among others. Using an animation property keyterm, while not prohibited by the spec, will likely break your animation when using the `animation` shorthand property discussed below, or even the `animation-name` property in the case of `none`.

After declaring the name of our @keyframe animation, we encompass all the rules of our at-rule in curly braces. This is where we will put all our keyframes.

### Keyframe Selectors

Keyframes selectors provide points during our animation where we set the values of the properties we want to animate. The entire at-rule specifies the behavior of one full iteration of the animation. The animation may iterate one or more times, or even less than one time.

The keyframe selectors consist of a comma-separated list of one or more percentage values or the keywords `from` or `to`. The keyword `from` is equal to `0%`. The keyword `to` equals `100%`.  The selector is used to specify the percentage along the duration of the animation that the keyframe represents. The keyframe itself is specified by the block of property values declared on the selector. The `%` unit must be used on percentage values: in other words, `0` is invalid as a keyframe selector.

    @keyframes W {
    	from {
    	 	left: 0;
    	  top: 0;
    	}
    	25%, 75% {
    	 	top: 100%;
    	}
    	50% {
    		top: 50%;
    	}
    	to {
    	left: 100%;
    		top: 0;
    	}
    }

In the above example, which would move a relatively or absolutely positioned element along a W shaped path if applied, has 5 keyframes at the 0%, 25%, 50%, 75% and 100% mark. The `from` is the 0% mark, the `to` is the 100% mark, and, as the property values we set for both the 25% and 75% mark is the same, we put two together as a comma-separated list of keyframe selectors.

Note that with the 25% and 75% on the same line, the selectors are not listed in order: they don't need to be. For ease of legibility, it is highly encouraged to progress from the 0% to the 100% mark, but it is not required.

If a `0%` or `from` keyframe is not specified, then the user agent constructs a `0%` keyframe using the original values of the properties being animated: as if the 0% keyframe were declared with the property values when no animation was applied. Similarly, if the `100%` or `to` keyframe is not defined, the browser creates a faux `100%` keyframe using the value the element would have had had no animation been set on it.

Assuming we have a background color change animation:

	@keyframes change_bgcolor {
    45% { background-color: green; }
    55% { background-color: blue; }
	}

And the element originally had it's background-color set to red, it would be as if the animation were written as:

	@keyframes change_bgcolor {
    0%   { background-color: red;}
    45%  { background-color: green; }
    55%  { background-color: blue; }
    100% { background-color: red;}
	}
or, remembering that we can including multiple, identical keyframes as a comma separated list :

	@keyframes change_bgcolor {
    0%, 100% { background-color: red;}
    45% { background-color: green; }
    55% { background-color: blue; }
	}

Negative values or values greater than `100%` are not valid and will be ignored.

In the original -webkit- implementation of animation, each keyframe could only be declared once: if declared more than once, only the last declaration would be applied, and the previous keyframe selector block was ignored. This has been updated. Now, similar to the rest of CSS, the values in the keyframe declaration blocks with identical keyframe values cascade. In the standard (non prefixed) syntax, the `W` animation above can be written with the `to`, or `100%`, declared twice, overriding the value of the left property:

    @keyframes W {
      from, to {
        top: 0;
        left: 0;
      }
      25%, 75% {
        top: 100%;
      }
      50% {
        top: 50%;
      }
      to {
        left: 100%;
      }
    }

Only animate animatable properties. Like the rest of CSS, properties and values in a keyframe declaration block that are not understood, are ignored. Properties that are not animatable are also ignored (with the exception of `animation-timing-function`).

> Note: The `animation-timing-function`, described in greater detail below, white not an animatable property, is not ignored. If you include the `animation-timing-function` as a keyframe style rule within a keyframe selector block, the `animation-timing-function` will change to that timing function when the animation moves to the next keyframe.

Do not try to animate between non-numeric values. You can animate between values that are written in a non-numeric way, as long as they can be extrapolated into a numberic value, like named colors which are extrapolated to hexadecimal color values.

If the animation is set between two property values that don't have a mid-point, as the results may not be what you expect: the property will not animated correctly or at all. For example, you can't animate between height: auto; and height: 300px; There is no mid-point between auto and 300px. The element may still animate, but different browsers handle this differently: Firefox does not animate the element. Safari may animate as if auto is equal to 0, and both Opera and Chrome currently jump from the pre animated state to the post animated state half way thru the animation, which may or may not be at the 50% keyframe selector, depending on your animation timing function.

Different browsers behave in differently for different properties when there is no midpoint, the behavior of your animation will be most predictable if you declare both a 0% and a 100% for every property you animate. For example, if you declare `border-radius: 50%;` in your animation, declare `border-radius: 0;` as-well, because there is no mid-point between `none` and anything: the default value of `border-radius` is `none`, not `0`.

That being said, not all the properties need to be included in each keyframe block. As long as an animatable property is included in at-least one block with a value that is different then the non-animated attribute value, and there is a possible midpoint between those two values, that property will animate.

## Animated Elements

Once you have created a keyframe animation, you need to apply that animation to an element or pseudo element for anything to actually animate. CSS Animation provides us with numerous animation properties to attach a keyframe animation to an element and control its progression. At minimum, we need to include the name of the animation for element to animate, and a duration if we want the animation to actually be visible.

### The `animation-name` property

The `animation-name` property takes as it's value the name or comma-separated names of the keyframe animation(s) you want to apply to that element.  The names are the unquoted identifiers you created in your @keyframes rule.

#### animation-name

 **Values:**

    &lt;@keyframes_identifier> | none | inherit | initial

 **Initial value:**

    none

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No

The default value is `none`, which means then there is no animation. This value can be used to override any animation applied elsewhere in the CSS cascade. To apply an animation, include the @keyframe identifier. To apply more than one animation, include more than one comma-separated @keyframe identifiers. If one of the included keyframe identifiers does not exist, the series of animation will not fail: rather, the failed animation will be ignored (and will be applied if that identifier does come into existance as a valid animation), and the other ones will be applied. 

If more than one animation is applied to an element that have repeated properties, the latter animations override the property values in the preceding animations. See Animation, Specificity and Precedence Order.

If you include three animation names, all the following properties such as `animation-duration` and `animation-iteration-count` should have three values as well, so that there are corresponding values for each attached animation. If there are too many values, the extra values are ignored. If there are too few comma separated values, the provided values will be repeated.

> If an included keyframe identifiers doesn't exist, the animation doesn't fail. Any other animations attached via the animation-name property will proceed normally. If that non-existant animation comes into existance, the animation will be attached to that element when the identifier becomes valid and will start iterating immediately or after the expiration of any `animation-delay`.

### The `animation-duration` property`

The `animation-duration` property defines how long a single animation iteration should take in seconds or milliseconds.

#### animation-duration

 **Values:**

    &lt;time>

 **Initial value:**

    0s

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No

The `animation-duration` property takes as it's value the length of time, in seconds (s) or milliseconds (ms), it should take to complete one cycle thru all the keyframes. If omitted, the animation will still be applied with a duration of 0s, with animationStart and animationEnd being fired. However, as the animation will take 0s to complete, it will be imperceptible. Negative values are invalid, and will behave as if the default of 0s were applied.


### The `animation-iteration-count` property

Include the `animation-iteration-count` property if you want to iterate through the animation more than the default one time. By default, if you simply include the required `animation-name`, the animation will play once.  

#### animation-iteration-count

 **Values:**

    &lt;number> | infinite

 **Initial value:**

    1

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No


By default, the animation will occur once. If included, the animation will repeat the number of times specified by the value if the `animation-iteration-count` property, which can be any number or the keyterm `infinite`. If the number is not an integer, the animation will end partway through its last cycle. Negative numbers are not valid, leading to a default single iteration.

Interestingly, 0 is a valid value for the `animation-iteration-count` property When set to 0, the animation still occurs over 0s, similar to setting `animation-duration: 0s;`, throwing both an `animationstart` and an `animationend` event. 

If the animation is not an integer, the animation will still run, but will cut off mid iteration on the final iteration. For example, `animation-iteration-count: 1.25` will iterate thru the animation 1.25 times, cutting off 25% thru the second iteration.


### The `animation-direction` property

With the `animation-direction` property you can control whether the animation progresses from the 0% keyframe to the 100% keyframe, or from the 100% keyframe to the 0% keyframe. You can control whether all the iterations progress in the same direction or set every other animation cycle to progress in the reverse direction. 

#### animation-iteration-direction

 **Values:**

    normal | reverse | alternate | alternate-reverse

 **Initial value:**

    normal

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No

When set to `normal`, each iteration of the animation progress from the 0% keyframe to the 100% keyframe.  are played as specified.
The `reverse` value sets each iteration to play in reverse keyframe order, progressing from the 100% keyframe to the 0% keyframe. The `alternate` value means that the first iteration (and each subsequent odd count iteration) should proceed from 0% to 100%, the second iteration (and each subsequent even numbered cycle) should reverse direction, proceeding from 100% to 0%. The `alternate-reverse` value is similar to the `alternate` value, except the odd numbered iterations are in the reverse direction, and the even numbered animation iterations are in the normal, or 0% to 100%; direction.

When an animation is played in reverse the timing functions is reversed. For example, when played in reverse an `ease-in` animation would appear to be an `ease-out` animation, progressing from the 100% keyframe to the 0% keyframe.


### The `animation-delay` property

The `animation-delay` property how long the browser waits after the animation is attached to the element before beginning the first animation iteration. The default value is 0s, meaning the animation will commence immediately when it is applied. A positive value will delay the start of the animation until the prescribed time listed as the value of the `animation-delay` property as elapsed. A negative value will cause the animation to begin immediately part way thru the animation.

#### animation-delay

 **Values:**

    &lt;time>

 **Initial value:**

    0s

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No


The time, defined in seconds (s) or milliseconds (ms) defines the delay between the application of the animation to the element and when the animation begins executing. By default, the animation begins iterating as soon as it is applied to the element, with a 0s delay. 

Including a negative value for the `animation-delay` property is valid, and can create interesting effects. A negative delay will execute the animation  immediately, but will begin the animating the element part way thru the attached animation. For example, if `animation-delay: -4s;` and `animation-duration: 10s;` are set on an element, the animation will begin immediately, but will start 40% of the way thru the first animation, which is not necessarily the 40% keyframe block: this depends on the value of the `animation-timing-function`. 

    div {
      animation-name: move;
      animation-duration: 10s;
      animation-delay: -4s;
    }
    @keyframes move {
      from { transform: translateX(0); }
      to { transform: translateX(1000px); }
    }

In the above example, for a `linear` animation, the animation would start with the div translated 400px to the right of its original position. 

### The `animation-timing-function` property

Similar to the  `transition-timing-function` property, the `animation-timing-function` property describes how the animation will progress over one cycle of its duration. 

#### animation-timing-function

 **Values:**

    ease | linear | ease-in | ease-out | |ease-in-out | step-start | step-end | steps(&lt;integer>, &lt;start|end>), cubic-bezier(&lt;number>, &lt;number>, &lt;number>, &lt;number>)

 **Initial value:**

    ease

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No


Other than the step timing functions, the timing functions are Bezier curves. Bezier curves are mathematically defined curves used in two-dimensional graphic applications. The curve is defined by four points: the initial position and the terminating position (which are called "anchors") and two separate middle points (which are called "handles"). In CSS, the anchors are at 0, 0 and 1, 1. While you can define your own bezier curve, there are 5 pre-defined bezier curvers. 

[PUT PICS OF BEZIER CURVES HERE]

The default `ease` is equal to cubic-bezier(0.25, 0.1, 0.25, 1), which This function is similar to `ease-in-out` at cubic-bezier(0.42, 0, 0.58, 1), though it accelerates more sharply at the beginning. `linear`is equal to cubic-bezier(0, 0, 1, 1), and, as the name describes, creates an animation that animates at a constant speed..  `ease-in` is equal to cubic-bezier(0.42, 0, 1, 1) which creates an animation that is slow to start, but gains speed, then stops  abruptly. The opposite `ease-out` timing function is equal to cubic-bezier(0, 0, 0.58, 1), starting and full spped, then slowing progressively as it reaches the conclusion of the animation iteration. If none of these work for you, you can create your own bezier curve timing function by passing 4 values:

    cubic-bezier(0.2, 0.4, 0.6, 0.8)

A bezier-curve takes four values. The first two at the x and y of the first point on the curve, and the last two are the x and y of the second point on the curve. The x values must between 0 and 1, or the cubic bezier is invalid. 

The step timing function step-start
The step-start function is equivalent to steps(1, start).
step-end
The step-end function is equivalent to steps(1, end).
steps(&lt;integer>[, [ start | end ] ]?)


Specifies a stepping function, described above, taking two parameters. The first parameter specifies the number of intervals in the function. It must be a positive integer (greater than 0). The second parameter, which is optional, is either the value ‘start’ or ‘end’, and specifies the point at which the change of values occur within the interval. If the second parameter is omitted, it is given the value ‘end’.
cubic-bezier(&lt;number>, &lt;number>, &lt;number>, &lt;number>)
Specifies a cubic-bezier curve. The four values specify points P1 and P2 of the curve as (x1, y1, x2, y2). Both x values must be in the range [0, 1] or the definition is invalid. The y values can exceed this range.

The values and meaning of &lt;dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-timing-function>&lt;single-timing-function&gt;`(#typedef-single-timing-function)&lt;/dfn> are identical to those of `">&lt;single-transition-timing-function&gt;`(http://dev.w3.org/csswg/css-transitions-1/#single-transition-timing-function title= "&lt;single-transition-timing-function") `CSS3-TRANSITIONS`(#css3-transitions title=css3-transitions "css3-transitions").

The timing function specified applies to e ach iteration of the animation, not the entire animation in full. For example, if an animation has `animation-timing-function: ease-in-out; animation-iteration-count: 2;`, it will ease in at the start, ease out as it approaches the end of its first iteration, ease in at the start of its second iteration, and ease out again as it approaches the end of the animation.

&lt;p class=note>  Note: Unlike other animation properties, `animation-timing-function` has an effect when specified on an individual keyframe.


### The `animation-play-state` property

The `animation-play-state` property defines whether the animation is running or paused.

#### animation-play-state

 **Values:**

    running | paused

 **Initial value:**

    running

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No

When set to the default `running`, the animation proceeds as normal. If set to 
`paused`, the animation will be paused. When paused, the animation is still applied to the element. If The animation continues to apply to the element with the progress it had made before being paused. When unpaused (set back to running), it restarts from where it left off, as if the "clock" that controls the animation had stopped and started again.
If the property is set to paused during the delay phase of the animation, the delay clock is also paused and resumes as soon as animation-play-state is set back to running.

### The `animation-fill-mode` property

The `animation-fill-mode` property defines what values are applied by the animation before and after the animation iterations are executed.  With this property we are able to define what values are applied by the animation outside the time it is executing.  By default, an animation will not affect property values of the element on which it is attached until after the animation delay has expired up until the last iteration has completed. We can control this with the  `animation-fill-mode` property. 

The  `animation-fill-mode` property enables us to apply the property values of the first keyframe to an element as soon as the animation is applied to that element thru the animation delay. It also enables us to maintain the property values of the last keyframe after the last animation cycle is complete.

#### animation-fill-mode

 **Values:**

    none | forwards | backwards | both

 **Initial value:**

    none

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No


none | forwards | backwards | both

The default value is `none`, which means the animation has no effect when it is not executing. The value of `forwards` means when the animation is done executing -- has concluded the later iteration as defined by the animation-iteration-count value - it continues to apply the values of the last keyframe. this is either the 100% keyframe, or, if the last iteration was reversed, the 0% keyframe. 

If the `animation-iteration-count` is a float value, and the last iteration doesn't end on the 0% or 100% keyframe, which means the animation ended its execution partway through an animation cycle, in Safari, as per the current specification, a forwards fill will apply the values of the 100% keyframe. In the specification, and in Safari, it doesn't matter whether the last iteration was normal or reverse, or whether the animation ends on the 1% or 99% keyframe, `animation-fill-mode: forward;` will cause the animation to jump to the 100% frame, and stay there, no matter where the partial animation concluded. It doesn't matter if the animation direction of that last iteration was normal or reverse. 

XXXX check the specs to see if this has been updated.

Not according to the specification, but likely the specification will change, Firefox, IE, Chrome and Opera all stop and hold on the property values the element had when the animation stopped. For example, if we take the following code:

    @keyframes moveMe {
      0% {
        transform: translatex(0);
      }
      100% {
        transform: translatex(1000px);
      }
    }
    .moved {
      animation: moveMe 10s linear 0.6 forwards;
    }

The animation will only go thru 0.6 iterations. Being a linear, 10 second animation, it will stop at the 60% mark 6 seconds into the animation, when the element is translated 600px to the right. With animation-fill-mode set to forwards, as per the spec, the element should jump to being translated by 400px to be 1000px to the right of where it normally would have been, and stay there indefinitely or until the animation is detached from the moved element. Firefox, IE, Chrome and Opera will also stop the animation when it is translated 600px to the right. But, contrary to the current state of the specification, though logically and likely following the final state of the specification, those browsers will hold the moved element 600px to the right of it's original position, keeping it translated indefinitely, or until the animation is detached from the element.

The backwards value controls what happens to the element after the animation was attached to the element, and up until the animation delay expires and the animation starts executing. Before the animation starts executing (during the period specified by animation-delay), the animation applies the values that it will start the first iteration with. The values specified in the animation's 0% keyframe are applied if the animation-direction is normal or alternate immediately when the animation is attached. If the animation-direction is reverse or alternate-reverse, the property values of the 100% keyframe are used.

If the 0% or 100% keyframes are not explicitly defined, the browser uses the implied values for those keyframes.

The value of `both` simply means that both `forwards` and `backwards` fill will be applied. As soon as the animation is attached to an element, that element will assume the properties provided in the 0% keyframe (or 100% keyframe in animation direction is set to `reverse` or `alternate-reverse`). When the last iteration concludes, it will be as if the animation-fill-mode were set to forwards: if it was a full iteration in the normal direction, the property values of the 100% keyframe will be applied, etc.

XXXX Issue: What happens with animation-duration: 0; animation-iteration-count: infinite;? The animation is instantaneous, but there is no "last complete iteration". In particular, you can’t tell whether to use the 0% or 100% keyframe.

### The `animation` shorthand property

The `animation` shorthand property enables us to use one line instead of eight to define all the animation properties on an element. The animation properties are space separated, with the animation shorthand being a comma-separated list of space separated animation properties.


#### animation

 **Values:**

    none | &lt;series of individual animation properties>

    &lt;animation-duration> || &lt;animation-timing-function> || &lt;animation-delay> || &lt;animation-iteration-count> || &lt;animation-direction> || &lt;animation-fill-mode> || &lt;animation-play-state> || &lt;animation-name>

 **Initial value:**

    none 0s ease 0s 1 normal none running

 **Applies to:**

    all elements, ::before and ::after pseudo-elements

 **Inherited:**

    No


The animation shorthand takes as it value all the other animation properties above, including &lt;animation-name>, &lt;animation-duration>, &lt;animation-timing-function>, &lt;animation-delay>, &lt;animation-iteration-count>, &lt;animation-direction>, &lt;animation-fill-mode>, and &lt;animation-play-state>. 

    animation: 200ms ease-in 50ms forwards slidedown;

is the equivalent of:

    animation-name: slidedown;
    animation-duration: 200ms;
    animation-timing-function: ease-in;
    animation-delay: 50ms;
    animation-iteration-count: 1;
    animation-fill-mode: forwards;
    animation-direction: normal;
    animation-play-state: running;

or 

    animation: 200ms ease-in 50ms 1 normal running forwards slidedown;

We didn't have to declare all of the values in the animation shorthand: any values that aren't declared are set to the default values. The above line is long, and in this case, 3 of the properties are set to default, so are not necessary. 

It's important to remember that if you don't declare all 8 values, the ones you don't declare will get the default value for that property. The default values are:

    animation-name: none;
    animation-duration: 0s;
    animation-timing-function: ease;
    animation-delay: 0;
    animation-iteration-count: 1;
    animation-fill-mode: none;
    animation-direction: normal;
    animation-play-state: running;

The order of the shorthand is partially important. For example, there are two time properties: the first is always interpreted as the duration, the second, if present, is interpreted as the delay.

The placement of the `animation-name` can also be important. If you use an animation-property value as your animation identifier, which you shouldn't, the  animation-name should be placed as the _last_ property value in the animation shorthand. The first occurence of a keyword that is a valid value for any property other than animation-name, such as `ease`, and `running`, will be assumed to be part of the shorthand of the animation property they're normally associated with rather than the `animation-name`. 

    animation: paused 2s;

The above is the equivalent to 

    animation-name: none;
    animation-duration: 2s;
    animation-delay: 0;
    animation-timing-function: ease;
    animation-iteration-count: 1;
    animation-fill-mode: none;
    animation-direction: normal;
    animation-play-state: paused; 

Paused is a valid animation name. While it may seem that the animation named `paused` with a duration of 2s is being attached to the element or psuedo element, that is not what is happening. Because words within the shorthand animation are first checked against possible valid values of all animation properties other than animation-name first, the `paused` is being set as the value of the animation-play-state property. 

    animation: running 2s ease-in-out forwards;

The above is the equivalent to 

    animation-name: none;
    animation-duration: 2s;
    animation-delay: 0s;
    animation-timing-function: ease-in-out;
    animation-iteration-count: 1;
    animation-fill-mode: forwards;
    animation-direction: normal;
    animation-play-state: running; 

Likely the developer has a keyframe animation called running. The browser, however, sees the term and assigns it to the animation-play-state property rather than the animation-name property. With no animation name declared, there is no animation attached to the element. 

In light of this, `animation: 2s 3s 4s;` may seem valid, as if the following were being set:

    animation-name: 4s;
    animation-duration: 2s;
    animation-delay: 3s;

If we remembember from the keyframe identifier section above, 4s is _not_ a valid identifier. Identifiers can not start with a digit unless escaped. For this animation be valid, it would have had to be written as `animation: 2s 3s \34 s;`

To attach multiple animations to a single element or pseudo element, comma separate the animation declarations.

    .snowflake {
      animation: falling 3s ease-in 200ms 32 forwards,
                 spinning 1.5s linear 200ms 64;
    }

Our snowflake with fall while spinning for 96 seconds, spinning twice during each 3s fall. At the end of the last animation cycle, the snowflake will stay fixed on the last keyframe of the falling @keyframes animation. We declared 6 of the 8 animation properties for the falling animation and 5 for the spinng animation, separating the two animations with a comma.

We put the animation name first as it's easier to read that way, but because of the issue with animation property keywords being valid keyframe identifiers, it may not be best practices.  

It is fine, even a good idea, to use the animation shorthand. Just remember  the placement of animation-duration, delay and name within that shorthand are important and ommitted values default to their default values. Also, it is a good idea to not use any animation keyterms as your keyframe identifier. 


## Animation, Specificity and Precedence Order

In terms of specificity, and which property values get applied: when an animation is attached to an element, it is as if the property values of that keyframe animation were set inline, `&lt;div style="keyframe properties here">`. In general, the weight of a property attached with an ID selector 1-0-0 should take precedence over a property applied by an element selector 0-0-1. However, if that property value was changed via a keyframe animation, it will be applied as if that property/value pair were added as an inline style. A property added via a CSS animation, even if that animation was added on a CSS block that had very low specificity, will be applied to the element, in-spite of there being the same property applied to the same element via a more specific selector. Similar to when a property value is added with style="", if an !important is declared on a property value within the cascade, that will override the style that was added with an animation. (ex. http://codepen.io/estelle/pen/iDvBz). There is discussion in the CSS Working group of making animations override even `!important` property values.

That being said, don't include `!important` within your animation declaration block: the property/value upon which it is declared will be ignored.

If there are multiple animations specifying values for the same property, the property value from the last animation applied will override the previous animations.

    div {
      animation-name: red, green, blue;
      animation-duration: 11s, 9s, 6s;
    }

In the code example above, if red, green and blue are all keyframe animations that change the color property to their respective names, once the `animation-name` and `animation duration` properties are applied to all a &lt;div>, for the first 6 seconds the color will be blue, then green for 3 seconds, then red for 2 seconds, before returning to its default color. If `animation-fill-mode: both;` were added to the mix, the color would always be blue, as the last animation, or blue, overrides the previous green animation, which overrides the red first animation.

The default properties of an element are not impacted before the animation starts, and the properties return to their original values after the animation ends unless an `animation-fill-mode` value other than the default `none` has been set.

When an animation is attached to an element, the properties of animation keyframes only effect the element on which they are applied while the animation is iterating, which is the time after the `animation-delay` has expired, through the completion of the last animation iteration. Again, you can ensure the keyframe animation properties impact the element before the expiration of the `animation-delay` and after the last iteration ends with the `animation-fill-mode` property.

If the `display` property is set to `none` on an element, any animation iterating on that element or its descencdants will cease, as if the animation were detached from the element. Updating the `display` property back to a visible value will reattach all the animation properties, restarting the animation from scratch. 

    .snowflake {
      animation: spin 2s linear 5s 20;
    }

The snowflake will spin 20 times, each spin takes 2 seconds, with the first spin starting after 5 seconds. If the snowflake element's `display` property gets set to none after 15 seconds, it would have completed 5 spins before disappearing (5 second delay, then 5 spins at 2 seconds each). If the snowflake display property changes, making it visible again, a 5 second delay will elapse before it starts spinning 20 times. It makes no differnce how many animation cycles iterated before it disappeared from view the first time.

Note that CSS animations have the lowest priority on the UI thread. If you attach multiple animation on page load with positive values for `animation-delay`, the delays expire as prescribed, but the animations will not begin until the UI thread is available to animate. For example, if you have 20 animations set with an animation delays to start animating at one second intervals over 20 seconds, with their `animation-delay` properties set to 1s, 2s, 3s, 4s and so on, if the document or application takes a long time to load, with 11 seconds between the time the animated elements were loaded and the UI thread has finished drawing the page, the animation delays of the first 11 animations will have expired, and will all commence when the UI thread has become available. The remaining animations will each then begin animating at one second intervals.


> *** While you can use animations to create changing content, dynamically changing content can lead to seizures in some users. Always keep accessibility in mind, including the accessibility of your website to people with epilepsy and other seizure disorders. 

#### Printing Animations

While not actually "animating" on a printed piece of paper, when an animated element is printed, the relevant property values will be printed. When it comes to print, obviously you can't see the element animating on a piece of paper, but if the animation causes an element to have a border-radius of 50%, the printed element will have a border-radius of 50%. 

============
ABOVE IS WHAT ESTELLE WROTE,
BELOW IS THE SPECIFICATION
============
















In addition to the property-specific values listed in their definitions, all properties defined in this specification also accept the `initial` and `inherit` keyword as their property value.




The start time of an animation is the time at which the style applying the animation and the corresponding @keyframes rule are both resolved. If an animation is specified for an element but the corresponding @keyframes rule does not yet exist, the animation cannot start; the animation will start from the beginning as soon as a matching @keyframes rule can be resolved. An animation specified by dynamically modifying the element’s style will start when this style is resolved; that may be immediately in the case of a pseudo style rule such as hover, or may be when the scripting engine returns control to the browser (in the case of style applied by script). Note that dynamically updating keyframe style rules does not start or restart an animation.

An animation applies to an element if its name appears as one of the identifiers in the computed value of the `animation-name` property and the animation uses a valid @keyframes rule. Once an animation has started it continues until it ends or the `animation-name` is removed. The values used for the keyframes and animation properties are snapshotted at the time the animation starts. Changing them during the execution of the animation has no effect. Note also that changing the value of `animation-name` does not necessarily restart an animation (e.g., if a list of animations are applied and one is removed from the list, only that animation will stop; The other animations will continue). In order to restart an animation, it must be removed then reapplied.

The end of the animation is defined by the combination of the `animation-duration`, `animation-iteration-count` and `animation-fill-mode` properties.

  div {
    animation-name: diagonal-slide;
      animation-duration: 5s;
      animation-iteration-count: 10; }

  @keyframes diagonal-slide {

      from {
        left: 0;
        top: 0;
      }

      to {
        left: 100px;
        top: 100px;
      }
    }

This will produce an animation that moves an element from (0, 0) to   (100px, 100px) over five seconds and repeats itself nine times  (for a total of ten iterations).







Issue: Need to describe what happens if a property is not present in all keyframes.

The @keyframes rule that is used by an animation will be the last one encountered in sorted rules order that matches the name of the animation specified by the `animation-name` property.
 div {
  		animation-name: slide-right;
  		animation-duration: 2s; }
 @keyframes slide-right {

  		from {
    		margin-left: 0px;
  		}

  		50% {
    		margin-left: 110px;
    		opacity: 1;
  		}

  		50% {
     	opacity: 0.9;
  		}

  		to {
    		margin-left: 200px;
  		}
 }

At the 1s mark, the slide-right animation will have the same state as if we had defined the 50% rule like this:

 @keyframes slide-right {

  		50% {
    		margin-left: 110px;
    		opacity: 0.9;
  		}

  		to {
    		margin-left: 200px;
  		}
 }


To determine the set of keyframes, all of the values in the selectors are sorted in increasing order by time. The rules within the @keyframes rule then cascade; the properties of a keyframe may thus derive from more than one @keyframes rule with the same selector value.

If a property is not specified for a keyframe, or is specified but invalid, the animation of that  property proceeds as if that keyframe did not exist. Conceptually, it is as if a set of keyframes is constructed for each property that is present in any of the keyframes, and an animation is run independently for each property.
 @keyframes wobble {
  		0% {
    		left: 100px;
  		}

  		40% {
    		left: 150px;
  		}

  		60% {
    		left: 75px;
  		}

  		100% {
    		left: 100px;
  		} }
  Four keyframes are specified for the animation named "wobble". In the first keyframe, 	shown at the beginning of the animation cycle, the value of the `left` property being animated is `100px`. By 40% of the animation duration, `left` has animated to `150px`. 	At 60% of the animation duration, `left` has animated back to `75px`. At the end of the animation cycle, the value of `left` has returned to `100px`. The diagram below shows the state of the animation if it were given a duration of `10s`.


### Timing functions for keyframes

A keyframe style rule may also declare the timing function that is to be used as the animation moves to the next keyframe.

	@keyframes bounce {
 		from {
	 		top: 100px;
		  animation-timing-function: ease-out;
		}
		25% {
  			top: 50px;
  			animation-timing-function: ease-in;
  		}
  		50% {
    		top: 100px;
    		animation-timing-function: ease-out;
    	}
    	75% {
    		top: 75px;
    		animation-timing-function: ease-in;
    	to {
    		top: 100px;
  		}
	}

Five keyframes are specified for the animation named "bounce". Between the first and second 	keyframe (i.e., between 0% and 25%) an ease-out timing function is used. Between the second  and third keyframe (i.e., between 25% and 50%) an ease-in timing function is used. And so on.

The effect will appear as an element that moves up the page 50px, slowing down as it reaches its highest point then speeding up as it falls back to 100px. The second half of the animation behaves in a similar manner, but only moves the element 25px up the page.

A timing function specified on the `to` or `100%` keyframe is ignored.

See the `animation-timing-function` property for more information.




## Animation Events

Several animation-related events are available through the DOM Event system. The start and end of an animation, and the end of each iteration of an animation, all generate DOM events. An element can have multiple properties being animated simultaneously. This can occur either with a single `animation-name` value with keyframes containing multiple properties, or with multiple `animation-name` values. For the purposes of events, each `animation-name` specifies a single animation. Therefore an event will be generated for each `animation-name` value and not necessarily for each property being animated.

Any animation for which a valid keyframe rule is defined will run and generate events; this includes animations with empty keyframe rules.

The time the animation has been running is sent with each event generated. This allows the event handler to determine the current iteration of a looping animation or the current position of an alternating animation. This time does not include any time the animation was in the `paused` play state.

### The `AnimationEvent` Interface

The `AnimationEvent` interface provides specific contextual information associated with Animation events.


#### Attributes

`animationName`, of type DOMString, readonly
The value of the animation-name property of the animation that fired the event.

`elapsedTime`, of type float, readonly
The amount of time the animation has been running, in seconds, when this event fired, excluding any time the animation was paused. For an animationstart event, the elapsedTime is zero unless there was a negative value for animation-delay, in which case the event will be fired with an elapsedTime of (-1 * delay).
`pseudoElement`, of type DOMString, readonly
The name (beginning with two colons) of the CSS pseudo-element on which the animation runs (in which case the target of the event is that pseudo-element’s corresponding element), or the empty string if the animation runs on an element (which means the target of the event is that element).
AnimationEvent(type, animationEventInitDict) is an event constructor.

### Types of AnimationEvent

The different types of animation events that can occur are:

** animationstart **

The `animationstart` event occurs at the start of the animation. If there is an animation-delay then this event will fire once the delay period has expired.

A negative delay will cause the event to fire with an elapsedTime equal to the absolute value of the delay; in this case the event will fire whether animation-play-state is set to running or paused.

Bubbles: Yes
Cancelable: No
Context Info: animationName, pseudoElement


** animationend ** 

The `animationend` event occurs when the animation finishes.

Bubbles: Yes
Cancelable: No
Context Info: animationName, elapsedTime, pseudoElement


** animationiteration **
The `animationiteration` event occurs at the end of each iteration of an animation, except when an animationend event would fire at the same time. This means that this event does not occur for animations with an iteration count of one or less.

Bubbles: Yes
Cancelable: No
Context Info: animationName, elapsedTime, pseudoElement

## 6 DOM Interfaces

CSS animations are exposed to the CSSOM through a pair of new interfaces describing the keyframes.

### The CSSRule Interface

The following two rule types are added to the CSSRule interface. They provide identification for the new keyframe and keyframes rules.

### The CSSKeyframeRule Interface

The CSSKeyframeRule interface represents the style rule for a single key.

#### Attributes

`keyText`, of type DOMString
This attribute represents the keyframe selector as a comma-separated list of percentage values. The from and to keywords map to 0% and 100%, respectively.
If keyText is updated with an invalid keyframe selector, a SyntaxError exception must be thrown.

`style`, of type CSSStyleDeclaration
This attribute represents the style associated with this keyframe.

### The CSSKeyframesRule Interface

The CSSKeyframesRule interface represents a complete set of keyframes for a single animation.

#### Attributes

`name`, of type DOMString
This attribute is the name of the keyframes, used by the animation-name property.

`cssRules`, of type CSSRuleList
This attribute gives access to the keyframes in the list.

#### The `appendRule` method

The `appendRule()` method appends the passed CSSKeyframeRule into the list at the passed key.

Parameters:

rule of type DOMString
The rule to be appended, expressed in the same syntax as one entry in the @keyframes rule.
No Return Value

No Exceptions

#### The deleteRule method

The `deleteRule()` deletes the CSSKeyframeRule with the passed key. If a rule with this key does not exist, the method does nothing.

Parameters:

key of type DOMString
The key which describes the rule to be deleted. A percentage value between 0% and 100%, or one of the keywords fro or to which resolve to 0% and 100%, respectively.
No Return Value

No Exceptions

#### The findRule method

The findRule() returns the rule with a key matching the passed key. If no such rule exists, a null value is returned.

Parameters:

key of type DOMString
The key which describes the rule to be deleted. A percentage value between 0% and 100%, or one of the keywords fro or to which resolve to 0% and 100%, respectively.
Return Value:

CSSKeyframeRule
The found rule.
No Exceptions


