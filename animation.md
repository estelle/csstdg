## <span class=content>Abstract</span>

This CSS module describes a way for authors to animate the values of CSS properties over time, using keyframes. The behavior of these keyframe animations can be controlled by specifying their duration, number of repeats, and repeating behavior.
[CSS](http://www.w3.org/TR/CSS/) is a language for describing the rendering of structured documents 
(such as HTML and XML) 
on screen, on paper, in speech, etc.

## <span class=content>Status of this document</span>

<div data-fill-with=status>

	This is a public copy of the editors’ draft. 
	It is provided for discussion only and may change at any moment. 
	Its publication here does not imply endorsement of its contents by W3C. 
	Don’t cite this document other than as work in progress.

<p>
	The ([archived](http://lists.w3.org/Archives/Public/www-style/)) public mailing list
	[www-style@w3.org](mailto:www-style@w3.org?Subject=%5Bcss-animations%5D%20PUT%20SUBJECT%20HERE) 
	(see [instructions](http://www.w3.org/Mail/Request)) 
	is preferred for discussion of this specification. 
	When sending e-mail, 
	please put the text “css-animations” in the subject, 
	preferably like this:
	“[css-animations] _…summary of comment…_”

<p>
	This document was produced by the [CSS Working Group](http://www.w3.org/Style/CSS/members) 
	(part of the [Style Activity](http://www.w3.org/Style/)).

<p>
	This document was produced by a group operating under 
	the [5 February 2004 W3C Patent Policy](http://www.w3.org/Consortium/Patent-Policy-20040205/). 
	W3C maintains a [public list of any patent disclosures](http://www.w3.org/2004/01/pp-impl/32061/status rel=disclosure) 
	made in connection with the deliverables of the group; 
	that page also includes instructions for disclosing a patent. 
	An individual who has actual knowledge of a patent which the individual believes contains [Essential Claim(s)](http://www.w3.org/Consortium/Patent-Policy-20040205/#def-essential) 
	must disclose the information in accordance with [section 6 of the W3C Patent Policy](http://www.w3.org/Consortium/Patent-Policy-20040205/#sec-Disclosure).</div>
<div data-fill-with=at-risk><p>The following features are at-risk, and may be dropped during the CR period:

</div>

## <span class=content>Table of Contents</span>

<div data-fill-with=table-of-contents>

</div>

## <span class=secno>1 </span><span class=content>
Introduction</span>[](#intro)

<p>	_This section is not normative_

<p>	CSS Transitions [[CSS3-TRANSITIONS]](#css3-transitions title=css3-transitions "css3-transitions") provide a way to interpolate
	CSS property values when they change as a result of underlying
	property changes. This provides an easy way to do simple animation,
	but the start and end states of the animation are controlled by the
	existing property values, and transitions provide little control to
	the author on how the animation progresses.

<p>	This proposal introduces defined animations, in which the author can
	specify the changes in CSS properties over time as a set of keyframes.
	Animations are similar to transitions in that they change the
	presentational value of CSS properties over time. The principal difference
	is that while transitions trigger implicitly when property values change,
	animations are explicitly executed when the animation properties are applied.
	Because of this, animations require explicit values for the properties
	being animated. These values are specified using animation keyframes,
	described below.

<p>	Many aspects of the animation can be controlled, including how many times
	the animation iterates, whether or not it alternates between the begin and
	end values, and whether or not the animation should be running or paused.
	An animation can also delay its start time.

## <span class=secno>2 </span><span class=content>
Values</span>[](#values)

<p>	This specification follows the CSS property definition conventions
	from [[CSS21]](#css21 title=css21 "css21"). Value types not defined in this specification are
	defined in CSS Level 2 Revision 1 [[CSS21]](#css21 title=css21 "css21"). Other CSS modules may
	expand the definitions of these value types: for example [[CSS3VAL]](#css3val title=css3val "css3val"),
	when combined with this module, expands the definition of the
	[">&lt;length&gt;](http://dev.w3.org/csswg/css-values-3/#length-value title= "<length") value type as used in this specification.

<p>	In addition to the property-specific values listed in their definitions,
	all properties defined in this specification also accept the ‘initial’
	and ‘inherit’ keyword as their property value. For readability it has
	not been repeated explicitly.

## <span class=secno>3 </span><span class=content>
Animations</span>[](#animations)

<p>	CSS Animations affect computed property values. This effect happens by
	adding a specified value to the CSS cascade ([[CSS3CASCADE]](#css3cascade title=css3cascade "css3cascade")) (at the
	level for CSS Animations) that will produce the correct computed value
	for the current state of the animation. As defined in [[CSS3CASCADE]](#css3cascade title=css3cascade "css3cascade"),
	animations override all normal rules, but are overridden by !important
	rules.

<p>	If at one point in time there are multiple animations specifying behavior
	for the same property, the animation which occurs last in the value
	of [animation-name](#propdef-animation-name title=animation-name "animation-name") will override the other animations at that point.

<p>	An animation does not affect the computed value before the application of
	the animation, before the animation delay has expired, and after the end of
	the animation.

<p>	While running, the animation computes the value of those properties
	it animates. Other values may take precedence over the animated value
	according to the CSS cascade ([[CSS3CASCADE]](#css3cascade title=css3cascade "css3cascade")). 

<p>	The start time of an animation is the time at which the style applying
	the animation and the corresponding @keyframes rule are both resolved.
	If an animation is specified for an element but the corresponding
	@keyframes rule does not yet exist, the animation cannot start; the 
	animation will start from the beginning as soon as a matching @keyframes 
	rule can be resolved. An animation specified by dynamically modifying the 
	element’s style will start when this style is resolved; that may be 
	immediately in the case of a pseudo style rule such as hover, or may be 
	when the scripting engine returns control to the browser (in the case of 
	style applied by script). Note that dynamically updating keyframe style 
	rules does not start or restart an animation.

<p>	An animation applies to an element if its name appears as one of the
	identifiers in the computed value of the [animation-name](#propdef-animation-name title=animation-name "animation-name") property and the
	animation uses a valid @keyframes rule. Once an
	animation has started it continues until it ends or the [animation-name](#propdef-animation-name title=animation-name "animation-name") is
	removed. The values used for the keyframes and animation properties are
	snapshotted at the time the animation starts. Changing them during the
	execution of the animation has no effect. Note also that changing the value
	of [animation-name](#propdef-animation-name title=animation-name "animation-name") does not necessarily restart an animation (e.g., if a list
	of animations are applied and one is removed from the list, only that animation
	will stop; The other animations will continue). In order to restart an animation,
	it must be removed then reapplied.

<p>	The end of the animation is defined by the combination of the
	[animation-duration](#propdef-animation-duration title=animation-duration "animation-duration"), [animation-iteration-count](#propdef-animation-iteration-count title=animation-iteration-count "animation-iteration-count") and [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode")
	properties.

	<div class=example>
		<pre>div {
  animation-name: diagonal-slide;
  animation-duration: 5s;
  animation-iteration-count: 10;
}

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
</pre>
<p>		This will produce an animation that moves an element from (0, 0) to
		(100px, 100px) over five seconds and repeats itself nine times
		(for a total of ten iterations).

	</div>

<p>	Setting the [display](http://dev.w3.org/csswg/css2/visuren.html#propdef-display title=display "display") property to <span class=css data-link-for=display data-link-type=maybe title=none>none</span> will terminate any running animation applied
	to the element and its descendants. If an element has a [display](http://dev.w3.org/csswg/css2/visuren.html#propdef-display title=display "display") of <span class=css data-link-for=display data-link-type=maybe title=none>none</span>, updating
	[display](http://dev.w3.org/csswg/css2/visuren.html#propdef-display title=display "display") to a value other than <span class=css data-link-for=display data-link-type=maybe title=none>none</span> will start all animations applied to the element
	by the [animation-name](#propdef-animation-name title=animation-name "animation-name") property, as well as all animations applied to descendants
	with [display](http://dev.w3.org/csswg/css2/visuren.html#propdef-display title=display "display") other than <span class=css data-link-for=display data-link-type=maybe title=none>none</span>.

<p>	While authors can use animations to create dynamically changing content, dynamically
	changing content can lead to seizures in some users. For information on how to avoid
	content that can lead to seizures, see Guideline 2.3: Seizures: Do not design content
	in a way that is known to cause seizures ([[WCAG20]](#wcag20 title=wcag20 "wcag20")).

<p>	Implementations may ignore animations when the rendering medium is not interactive e.g. when printed.
	A future version of this specification may define how to render animations for these media.

## <span class=secno>4 </span><span class=content>
Keyframes</span>[](#keyframes)

<p>	Keyframes are used to specify the values for the animating properties at various points
	during the animation. The keyframes specify the behavior of one cycle of the animation;
	the animation may iterate one or more times.

<p>	Keyframes are specified using a specialized CSS at-rule. A <dfn class=css-code data-dfn-type=at-rule data-export="" id=at-ruledef-keyframes>@keyframes[](#at-ruledef-keyframes)</dfn> rule consists of the
	keyword "@keyframes", followed by an identifier giving a name for the animation (which will
	be referenced using [animation-name](#propdef-animation-name title=animation-name "animation-name")), followed by a set of style rules (delimited by curly
	braces).

<p>	The keyframe selector for a keyframe style rule consists of a comma-separated list of
	percentage values or the keywords <span class=css data-link-type=maybe title=from>from</span> or <span class=css data-link-type=maybe title=to>to</span>. The selector is used to specify the
	percentage along the duration of the animation that the keyframe represents. The keyframe
	itself is specified by the block of property values declared on the selector. The keyword
	<span class=css data-link-type=maybe title=from>from</span> is equivalent to the value <span class=css data-link-type=maybe title=0%>0%</span>. The keyword <span class=css data-link-type=maybe title=to>to</span> is equivalent to the value <span class=css data-link-type=maybe title=100%>100%</span>.
	<span class=note>Note that the percentage unit specifier must be used on percentage values.
	Therefore, <span class=css data-link-type=maybe title=0>0</span> is an invalid keyframe selector.</span>

<p>	If a <span class=css data-link-type=maybe title=0%>0%</span> or <span class=css data-link-type=maybe title=from>from</span> keyframe is not specified, then the user agent constructs a <span class=css data-link-type=maybe title=0%>0%</span> keyframe
	using the computed values of the properties being animated. If a <span class=css data-link-type=maybe title=100%>100%</span> or <span class=css data-link-type=maybe title=to>to</span> keyframe is not
	specified, then the user agent constructs a <span class=css data-link-type=maybe title=100%>100%</span> keyframe using the computed values of the
	properties being animated. If a keyframe selector specifies negative percentage values or values
	higher than <span class=css data-link-type=maybe title=100%>100%</span>, then the keyframe will be ignored.

<p>	The keyframe declaration block for a keyframe rule consists of properties and values. Properties
	that are unable to be animated are ignored in these rules, with the exception of
	[animation-timing-function](#propdef-animation-timing-function title=animation-timing-function "animation-timing-function"), the behavior of which is described below. In addition, keyframe rule
	declarations qualified with !important are ignored.

<p>	Issue: Need to describe what happens if a property is not present in all keyframes.

<p>	The @keyframes rule that is used by an animation will be the last one encountered in sorted rules
	order that matches the name of the animation specified by the [animation-name](#propdef-animation-name title=animation-name "animation-name") property. 

	<div class=example>
		<pre>div {
  animation-name: slide-right;
  animation-duration: 2s;
}

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
</pre>
<p>		At the 1s mark, the slide-right animation will have the same state as if we had defined the 50% rule like this:

		<pre>
@keyframes slide-right {

  50% {
    margin-left: 110px;
    opacity: 0.9;
  }

  to {
    margin-left: 200px;
  }

}
</pre>
	</div>

<p class=note>	Note: Since empty @keyframes rules are valid, they may hide the keyframes of those
	preceding animation definitions with a matching name. 

<p>	To determine the set of keyframes, all of the values in the selectors are sorted in increasing order
	by time. The rules within the @keyframes rule then cascade; the properties of a keyframe may thus derive
	from more than one @keyframes rule with the same selector value. 

<p>	If a property is not specified for a keyframe, or is specified but invalid, the animation of that
	property proceeds as if that keyframe did not exist. Conceptually, it is as if a set of keyframes is
	constructed for each property that is present in any of the keyframes, and an animation is run
	independently for each property.

	<div class=example>
		<pre>@keyframes wobble {
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
  }
}
</pre>
<p>		Four keyframes are specified for the animation named "wobble". In the first keyframe,
		shown at the beginning of the animation cycle, the value of the [left](http://dev.w3.org/csswg/css-position-3/#left title=left "left") property being
		animated is <span class=css data-link-type=maybe title=100px>100px</span>. By 40% of the animation duration, [left](http://dev.w3.org/csswg/css-position-3/#left title=left "left") has animated to <span class=css data-link-type=maybe title=150px>150px</span>.
		At 60% of the animation duration, [left](http://dev.w3.org/csswg/css-position-3/#left title=left "left") has animated back to <span class=css data-link-type=maybe title=75px>75px</span>. At the end of the
		animation cycle, the value of [left](http://dev.w3.org/csswg/css-position-3/#left title=left "left") has returned to <span class=css data-link-type=maybe title=100px>100px</span>. The diagram below shows
		the state of the animation if it were given a duration of <span class=css data-link-type=maybe title=10s>10s</span>.

<figure>
			![](./animation1.png)
			<figcaption>Animation states specified by keyframes</figcaption>
		</figure>
	</div>

	The following is the grammar for the keyframes rule:

	<pre>  keyframes_rule: KEYFRAMES_SYM S+ IDENT S* '{' S* keyframes_blocks '}' S*;

  keyframes_blocks: [ keyframe_selector '{' S* declaration? [ ';' S* declaration? ]* '}' S* ]* ;

  keyframe_selector: [ FROM_SYM | TO_SYM | PERCENTAGE ] S* [ ',' S* [ FROM_SYM | TO_SYM | PERCENTAGE ] S* ]*;

  @{K}{E}{Y}{F}{R}{A}{M}{E}{S}   {return KEYFRAMES_SYM;}
  {F}{R}{O}{M}                   {return FROM_SYM;}
  {T}{O}                         {return TO_SYM;}
</pre>

### <span class=secno>4.1 </span><span class=content>
Timing functions for keyframes</span>[](#timing-functions)

<p>	A keyframe style rule may also declare the timing function that is to be used as the animation
	moves to the next keyframe.

	<div class=example>
		<pre>@keyframes bounce {

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
  }

  to {
    top: 100px;
  }

}
</pre>
<p>		Five keyframes are specified for the animation named "bounce". Between the first and second
		keyframe (i.e., between 0% and 25%) an ease-out timing function is used. Between the second
		and third keyframe (i.e., between 25% and 50%) an ease-in timing function is used. And so on.
		The effect will appear as an element that moves up the page 50px, slowing down as it reaches
		its highest point then speeding up as it falls back to 100px. The second half of the animation
		behaves in a similar manner, but only moves the element 25px up the page.
	</div>

<p>	A timing function specified on the <span class=css data-link-type=maybe title=to>to</span> or <span class=css data-link-type=maybe title=100%>100%</span> keyframe is ignored.

<p>	See the [animation-timing-function](#propdef-animation-timing-function title=animation-timing-function "animation-timing-function") property for more information.

### <span class=secno>4.2 </span><span class=content>
The [animation-name](#propdef-animation-name title=animation-name "animation-name") property</span>[](#animation-name)

<p>	 The [animation-name](#propdef-animation-name title=animation-name "animation-name") property defines a list of animations that apply. Each name is used to select
	 the keyframe at-rule that provides the property values for the animation. If the name does not match
	 any keyframe at-rule, there are no properties to be animated and the animation will not execute.
	 Furthermore, if the animation name is `none` then there will be no animation. This can be
	 used to override any animations coming from the cascade. If multiple animations are attempting to
	 modify the same property, then the animation closest to the end of the list of names wins.

<p>	Each animation listed by name should have a corresponding value for the other animation properties
	listed below. If the lists of values for the other animation properties do not have the same length,
	the length of the [animation-name](#propdef-animation-name title=animation-name "animation-name") list determines the number of items in each list examined when
	starting animations. The lists are matched up from the first value: excess values at the end are not
	used. If one of the other properties doesn’t have enough comma-separated values to match the number of
	values of [animation-name](#propdef-animation-name title=animation-name "animation-name"), the UA must calculate its used value by repeating the list of values until
	there are enough. This truncation or repetition does not affect the computed value.

<p class=note>	Note: This is analogous to the behavior of the ‘background-*’properties, with ‘background-image’ analogous
	to [animation-name](#propdef-animation-name title=animation-name "animation-name").

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-name>animation-name[](#propdef-animation-name)</dfn><tr><th>Value:<td class=prod>[">&lt;single-animation-name&gt;](#typedef-single-animation-name title= "<single-animation-name")#<tr><th>Initial:<td>none<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>none<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
<p>	<dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-animation-name>&lt;single-animation-name&gt;[](#typedef-single-animation-name)</dfn> = none | [">&lt;custom-ident&gt;](http://dev.w3.org/csswg/css-values-3/#identifier-value title= "<custom-ident")

<p>	The values of [animation-name](#propdef-animation-name title=animation-name "animation-name") have the following meanings:

	<dl data-dfn-for=animation-name data-dfn-type=value>
		<dt><dfn class=css-code data-dfn-for=animation-name data-dfn-type=value data-export="" id=valuedef-none0>none[](#valuedef-none0)</dfn>
		<dd>
			No keyframes are specified at all, so there will be no animation.
			Any other animations properties specified for this animation have no effect.

		<dt><dfn class=css-code data-dfn-for=animation-name data-dfn-type=value data-export="" id=valuedef-custom-ident>[">&lt;custom-ident&gt;](http://dev.w3.org/csswg/css-values-3/#identifier-value title= "<custom-ident")[](#valuedef-custom-ident)</dfn>
		<dd>
			The animation will use the keyframes with the name specified by the [">&lt;custom-ident&gt;](http://dev.w3.org/csswg/css-values-3/#identifier-value title= "<custom-ident"),
			if they exist.
			If no such keyframes exist,
			there is no animation.
	</dl>

### <span class=secno>4.3 </span><span class=content>
The [animation-duration](#propdef-animation-duration title=animation-duration "animation-duration") property</span>[](#animation-duration)

<p>	The [animation-duration](#propdef-animation-duration title=animation-duration "animation-duration") property defines duration of a single animation cycle.

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-duration>animation-duration[](#propdef-animation-duration)</dfn><tr><th>Value:<td class=prod>[">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time")#<tr><th>Initial:<td>0s<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
	<dl>
		<dt><dfn class=css-code data-dfn-for=animation-duration data-dfn-type=value data-export="" id=valuedef-time0>[">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time")[](#valuedef-time0)</dfn>
		<dd>
			The [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") specifies the length of time that an animation takes to complete one cycle.
			A negative [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") is invalid.

<p>			If the [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") is <span class=css data-link-type=maybe title=0s>0s</span>, like the initial value,
			the keyframes of the animation have no effect,
			but the animation itself still occurs instantaneously.
			That is, [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") applies as normal, 
			filling backwards or forwards as appropriate; 
			start and end events are fired.
	</dl>

### <span class=secno>4.4 </span><span class=content>
The [animation-timing-function](#propdef-animation-timing-function title=animation-timing-function "animation-timing-function") property</span>[](#animation-timing-function)

<p>	The [animation-timing-function](#propdef-animation-timing-function title=animation-timing-function "animation-timing-function") property describes how the animation will progress over
	one cycle of its duration. See the [transition-timing-function](http://dev.w3.org/csswg/css-transitions-1/#transition-timing-function title=transition-timing-function "transition-timing-function") property [[CSS3-TRANSITIONS]](#css3-transitions title=css3-transitions "css3-transitions")
	for a complete description of timing function calculation.

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-timing-function>animation-timing-function[](#propdef-animation-timing-function)</dfn><tr><th>Value:<td class=prod>[">&lt;single-timing-function&gt;](#typedef-single-timing-function title= "<single-timing-function")#<tr><th>Initial:<td>ease<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
<p>	The values and meaning of <dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-timing-function>&lt;single-timing-function&gt;[](#typedef-single-timing-function)</dfn>
	are identical to those of [">&lt;single-transition-timing-function&gt;](http://dev.w3.org/csswg/css-transitions-1/#single-transition-timing-function title= "<single-transition-timing-function") [[CSS3-TRANSITIONS]](#css3-transitions title=css3-transitions "css3-transitions").

<p>	The timing function specified applies to each iteration of the animation,
	not the entire animation in full.
	For example, if an animation has [animation-timing-function: ease-in-out; animation-iteration-count: 2;](#propdef-animation-timing-function title=animation-timing-function "animation-timing-function"),
	it will ease in at the start,
	ease out as it approaches the end of its first iteration,
	ease in at the start of its second iteration,
	and ease out again as it approaches the end of the animation.

<p class=note>	Note: Unlike other animation properties,
	[animation-timing-function](#propdef-animation-timing-function title=animation-timing-function "animation-timing-function") has an effect when specified on an individual keyframe.
	See [](#timing-functions section=) for more detail on this.

### <span class=secno>4.5 </span><span class=content>
The [animation-iteration-count](#propdef-animation-iteration-count title=animation-iteration-count "animation-iteration-count") property</span>[](#animation-iteration-count)

<p>	The [animation-iteration-count](#propdef-animation-iteration-count title=animation-iteration-count "animation-iteration-count") property specifies the number of times an animation cycle
	is played. The initial value is <span class=css data-link-type=maybe title=1>1</span>, meaning the animation will play from beginning to end
	once.  This property is often used in conjunction with an
	[animation-direction](#propdef-animation-direction title=animation-direction "animation-direction") value of [alternate](#valuedef-alternate title=alternate "alternate"), which will cause the animation to play in
	reverse on alternate cycles.

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-iteration-count>animation-iteration-count[](#propdef-animation-iteration-count)</dfn><tr><th>Value:<td class=prod>[">&lt;single-animation-iteration-count&gt;](#typedef-single-animation-iteration-count title= "<single-animation-iteration-count")#<tr><th>Initial:<td>1<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
<p>	<dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-animation-iteration-count>&lt;single-animation-iteration-count&gt;[](#typedef-single-animation-iteration-count)</dfn> = infinite | [">&lt;number&gt;](http://dev.w3.org/csswg/css-values-3/#number-value title= "<number")

	<dl data-dfn-for=animation-iteration-count data-dfn-type=value>
		<dt><dfn class=css-code data-dfn-for=animation-iteration-count data-dfn-type=value data-export="" id=valuedef-infinite>infinite[](#valuedef-infinite)</dfn>
		<dd>
			The animation will repeat forever.

		<dt><dfn class=css-code data-dfn-for=animation-iteration-count data-dfn-type=value data-export="" id=valuedef-number>[">&lt;number&gt;](http://dev.w3.org/csswg/css-values-3/#number-value title= "<number")[](#valuedef-number)</dfn>
		<dd>
			<p>The animation will repeat the specified number of times.
			If the number is not an integer,
			the animation will end partway through its last cycle.
			Negative numbers are invalid. 

			<p>A value of <span class=css data-link-type=maybe title=0>0</span> is valid and, similar to an [animation-duration](#propdef-animation-duration title=animation-duration "animation-duration") 
			of <span class=css data-link-type=maybe title=0s>0s</span>, causes the animation to occur instantaneously. 
			Specifically, if [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") is set to [backwards](#valuedef-backwards title=backwards "backwards") or 
			[both](#valuedef-both title=both "both"), the first frame of the animation, as defined by 
			[animation-direction](#propdef-animation-direction title=animation-direction "animation-direction"), will be displayed during the 
			[animation-delay](#propdef-animation-delay title=animation-delay "animation-delay"). Then the last frame of the animation, 
			as defined by [animation-direction](#propdef-animation-direction title=animation-direction "animation-direction"), will be displayed if 
			[animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") is set to [forwards](#valuedef-forwards title=forwards "forwards") or [both](#valuedef-both title=both "both"). 
			If [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") is set to [none](#valuedef-none title=none "none") 
			then the animation has no effect. 

			<p class=issue id=issue-ef1c522a>[](#issue-ef1c522a)If similar to animation-duration:0s, also relates to whether
			animation events fire?
	</dl>

### <span class=secno>4.6 </span><span class=content>
The [animation-direction](#propdef-animation-direction title=animation-direction "animation-direction") property</span>[](#animation-direction)

<p>	The [animation-direction](#propdef-animation-direction title=animation-direction "animation-direction") property defines whether or not the animation should play in reverse
	on some or all cycles. When an animation is played in reverse the timing functions are also
	reversed. For example, when played in reverse an <span class=css data-link-type=maybe title=ease-in>ease-in</span> animation would appear to be an
	<span class=css data-link-type=maybe title=ease-out>ease-out</span> animation.

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-direction>animation-direction[](#propdef-animation-direction)</dfn><tr><th>Value:<td class=prod>[">&lt;single-animation-direction&gt;](#typedef-single-animation-direction title= "<single-animation-direction")#<tr><th>Initial:<td>normal<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
<p>	<dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-animation-direction>&lt;single-animation-direction&gt;[](#typedef-single-animation-direction)</dfn> = normal | reverse | alternate | alternate-reverse

	<dl data-dfn-for=animation-direction data-dfn-type=value>
		<dt><dfn class=css-code data-dfn-for=animation-direction data-dfn-type=value data-export="" id=valuedef-normal>normal[](#valuedef-normal)</dfn>
		<dd>
			All iterations of the animation are played as specified.

		<dt><dfn class=css-code data-dfn-for=animation-direction data-dfn-type=value data-export="" id=valuedef-reverse>reverse[](#valuedef-reverse)</dfn>
		<dd>
			All iterations of the animation are played in the reverse direction
			from the way they were specified.

		<dt><dfn class=css-code data-dfn-for=animation-direction data-dfn-type=value data-export="" id=valuedef-alternate>alternate[](#valuedef-alternate)</dfn>
		<dd>
			The animation cycle iterations that are odd counts are played in the
			normal direction, and the animation cycle iterations that are even
			counts are played in a reverse direction.

		<dt><dfn class=css-code data-dfn-for=animation-direction data-dfn-type=value data-export="" id=valuedef-alternate-reverse>alternate-reverse[](#valuedef-alternate-reverse)</dfn>
		<dd>
			The animation cycle iterations that are odd counts are played in the
			reverse direction, and the animation cycle iterations that are even
			counts are played in a normal direction.
	</dl>

<p class=note>	Note: For the purpose of determining whether an iteration is even or odd,
	iterations start counting from 1.

### <span class=secno>4.7 </span><span class=content>
The [animation-play-state](#propdef-animation-play-state title=animation-play-state "animation-play-state") property</span>[](#animation-play-state)

<p>	The [animation-play-state](#propdef-animation-play-state title=animation-play-state "animation-play-state") property defines whether the animation is running or paused.

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-play-state>animation-play-state[](#propdef-animation-play-state)</dfn><tr><th>Value:<td class=prod>[">&lt;single-animation-play-state&gt;](#typedef-single-animation-play-state title= "<single-animation-play-state")#<tr><th>Initial:<td>running<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
<p>	<dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-animation-play-state>&lt;single-animation-play-state&gt;[](#typedef-single-animation-play-state)</dfn> = running | paused

	<dl data-dfn-for=animation-play-state data-dfn-type=value>
		<dt><dfn class=css-code data-dfn-for=animation-play-state data-dfn-type=value data-export="" id=valuedef-running>running[](#valuedef-running)</dfn>
		<dd>
			While this property is set to [running](#valuedef-running title=running "running"),
			the animation proceeds as normal.

		<dt><dfn class=css-code data-dfn-for=animation-play-state data-dfn-type=value data-export="" id=valuedef-paused>paused[](#valuedef-paused)</dfn>
		<dd>
			While this property is set to [paused](#valuedef-paused title=paused "paused"),
			the animation is paused.
			The animation continues to apply to the element with the progress it had made before being paused.
			When unpaused (set back to [running](#valuedef-running title=running "running")), it restarts from where it left off,
			as if the "clock" that controls the animation had stopped and started again.

<p>			If the property is set to [paused](#valuedef-paused title=paused "paused") during the delay phase of the animation,
			the delay clock is also paused and resumes as soon as [animation-play-state](#propdef-animation-play-state title=animation-play-state "animation-play-state") is set back to [running](#valuedef-running title=running "running").
	</dl>

### <span class=secno>4.8 </span><span class=content>
The [animation-delay](#propdef-animation-delay title=animation-delay "animation-delay") property</span>[](#animation-delay)

<p>	The [animation-delay](#propdef-animation-delay title=animation-delay "animation-delay") property defines when the animation will start. It allows an animation
	to begin execution some time after it is applied,
	or to appear to have begun execution some time _before_ it is applied.

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-delay>animation-delay[](#propdef-animation-delay)</dfn><tr><th>Value:<td class=prod>[">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time")#<tr><th>Initial:<td>0s<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
	<dl>
		<dt><dfn class=css-code data-dfn-for=animation-delay data-dfn-type=value data-export="" id=valuedef-time>[">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time")[](#valuedef-time)</dfn>
		<dd>
			The [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") defines how long of a delay there is between the start of the animation
			(when the animation is applied to the element via these properties)
			and when it begins executing.
			A delay of <span class=css data-link-type=maybe title=0s>0s</span> (the initial value) means that the animation will execute as soon as it is applied.

<p>			A negative delay is **valid**.
			Similar to a delay of <span class=css data-link-type=maybe title=0s>0s</span>, it means that the animation executes immediately,
			but is automatically progressed by the absolute value of the delay,
			as if the animation had started the specified time in the past,
			and so it appears to start partway through its play-cycle already.
			If an animation’s keyframes have an implied starting value,
			the values are taken from the time the animation starts,
			not some time in the past.
	</dl>

### <span class=secno>4.9 </span><span class=content>
The [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") property</span>[](#animation-fill-mode)

<p>	The [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") property defines what values are applied by the animation
	outside the  time it is executing. By default, an animation will not affect property
	values between the time it is applied (the ‘animation-name’ property is set on an
	element) and the time it begins execution (which is determined by the [animation-delay](#propdef-animation-delay title=animation-delay "animation-delay")
	property). Also, by default an animation does not affect property values after the
	animation ends (determined by the [animation-duration](#propdef-animation-duration title=animation-duration "animation-duration") and [animation-iteration-count](#propdef-animation-iteration-count title=animation-iteration-count "animation-iteration-count") properties).
	The [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") property can override this behavior.

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation-fill-mode>animation-fill-mode[](#propdef-animation-fill-mode)</dfn><tr><th>Value:<td class=prod>[">&lt;single-animation-fill-mode&gt;](#typedef-single-animation-fill-mode title= "<single-animation-fill-mode")#<tr><th>Initial:<td>none<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
<p>	<dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-animation-fill-mode>&lt;single-animation-fill-mode&gt;[](#typedef-single-animation-fill-mode)</dfn> = none | forwards | backwards | both

	<dl data-dfn-for=animation-fill-mode data-dfn-type=value>
		<dt><dfn class=css-code data-dfn-for=animation-fill-mode data-dfn-type=value data-export="" id=valuedef-none>none[](#valuedef-none)</dfn>
		<dd>
			The animation has no effect when it is applied but not executing.

		<dt><dfn class=css-code data-dfn-for=animation-fill-mode data-dfn-type=value data-export="" id=valuedef-forwards>forwards[](#valuedef-forwards)</dfn>
		<dd>
			After the animation is done executing
			(has played the number of times specified by its [animation-iteration-count](#propdef-animation-iteration-count title=animation-iteration-count "animation-iteration-count") value)
			it continues to apply the values that it ended its last complete iteration with.
			This will be the values specified or implied for either its <span class=css data-link-type=maybe title=100%>100%</span> or <span class=css data-link-type=maybe title=0%>0%</span> keyframe,
			depending on the direction that the last complete iteration was executing in
			(per [animation-direction](#propdef-animation-direction title=animation-direction "animation-direction")).
			If the animation didn’t complete an entire iteration
			(if the iteration count was <span class=css data-link-type=maybe title=0>0</span> or a value less than 1)
			the values specified or implied for its <span class=css data-link-type=maybe title=0%>0%</span> keyframe are used.

<p class=note>			Note: If [animation-iteration-count](#propdef-animation-iteration-count title=animation-iteration-count "animation-iteration-count") is a non-integer value,
			the animation will stop executing partway through its animation cycle,
			but a forwards fill will still apply the values of the <span class=css data-link-type=maybe title=100%>100%</span> keyframe,
			not whatever values were being applied at the time the animation stopped executing.

<p>			Issue: Why does it ignore the progress made by a non-integer iteration count?

<p>			Issue: What happens with [animation-duration: 0; animation-iteration-count: infinite;](#propdef-animation-duration title=animation-duration "animation-duration")?
			The animation is instantaneous,
			but there is no "last complete iteration".
			In particular, you can’t tell whether to use the 0% or 100% keyframe.

		<dt><dfn class=css-code data-dfn-for=animation-fill-mode data-dfn-type=value data-export="" id=valuedef-backwards>backwards[](#valuedef-backwards)</dfn>
		<dd>
			Before the animation has begun executing
			(during the period specified by [animation-delay](#propdef-animation-delay title=animation-delay "animation-delay")),
			the animation applies the values that it will start the first iteration with.
			If the [animation-direction](#propdef-animation-direction title=animation-direction "animation-direction") is [normal](#valuedef-normal title=normal "normal") or [alternate](#valuedef-alternate title=alternate "alternate"),
			the values specified or implied for its <span class=css data-link-type=maybe title=0%>0%</span> keyframe are used;
			if the [animation-direction](#propdef-animation-direction title=animation-direction "animation-direction") is [reverse](#valuedef-reverse title=reverse "reverse") or [alternate-reverse](#valuedef-alternate-reverse title=alternate-reverse "alternate-reverse"),
			the values specified or implied for its <span class=css data-link-type=maybe title=100%>100%</span> keyframe are used.

		<dt><dfn class=css-code data-dfn-for=animation-fill-mode data-dfn-type=value data-export="" id=valuedef-both>both[](#valuedef-both)</dfn>
		<dd>
			The effects of both [forwards](#valuedef-forwards title=forwards "forwards") and [backwards](#valuedef-backwards title=backwards "backwards") fill apply.
	</dl>

### <span class=secno>4.10 </span><span class=content>
The [animation](#propdef-animation title=animation "animation") shorthand property</span>[](#animation)

<p>	The [animation](#propdef-animation title=animation "animation") shorthand property is a comma-separated list of animation definitions. Each item in
	the list gives one item of the value for all of the subproperties of the shorthand, which are known
	as the animation properties. (See the definition of [animation-name](#propdef-animation-name title=animation-name "animation-name") for what happens when these
	properties have lists of different lengths, a problem that cannot occur when they are defined using
	only the [animation](#propdef-animation title=animation "animation") shorthand.)

<table class="definition propdef"><tr><th>Name:<td><dfn class=css-code data-dfn-type=property data-export="" id=propdef-animation>animation[](#propdef-animation)</dfn><tr><th>Value:<td class=prod>[">&lt;single-animation&gt;](#typedef-single-animation title= "<single-animation")#<tr><th>Initial:<td>see individual properties<tr><th>Applies to:<td>all elements, ::before and ::after pseudo-elements<tr><th>Inherited:<td>no<tr><th>Media:<td>interactive<tr><th>Computed value:<td>As specified<tr><th>Canonical order:<td>per grammar<tr><th>Percentages:<td>N/A<tr><th>Animatable:<td>no</table>
<p>	<dfn class=css-code data-dfn-type=type data-export="" id=typedef-single-animation>&lt;single-animation&gt;[](#typedef-single-animation)</dfn> = [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") || [">&lt;single-timing-function&gt;](#typedef-single-timing-function title= "<single-timing-function") || [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") || [">&lt;single-animation-iteration-count&gt;](#typedef-single-animation-iteration-count title= "<single-animation-iteration-count") || [">&lt;single-animation-direction&gt;](#typedef-single-animation-direction title= "<single-animation-direction") || [">&lt;single-animation-fill-mode&gt;](#typedef-single-animation-fill-mode title= "<single-animation-fill-mode") || [">&lt;single-animation-play-state&gt;](#typedef-single-animation-play-state title= "<single-animation-play-state") || [">&lt;single-animation-name&gt;](#typedef-single-animation-name title= "<single-animation-name")

<p>	Note that order is important within each animation definition: the first value in each
	[">&lt;single-animation&gt;](#typedef-single-animation title= "<single-animation") that can be parsed as a [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") is assigned to the [animation-duration](#propdef-animation-duration title=animation-duration "animation-duration"),
	and the second value in each [">&lt;single-animation&gt;](#typedef-single-animation title= "<single-animation") that can be parsed as a [">&lt;time&gt;](http://dev.w3.org/csswg/css-values-3/#time-value title= "<time") is assigned to
	[animation-delay](#propdef-animation-delay title=animation-delay "animation-delay").

<p>	Note that order is also important within each animation definition for distinguishing
	[">&lt;single-animation-name&gt;](#typedef-single-animation-name title= "<single-animation-name") values from other keywords. When parsing, keywords that are valid for
	properties other than [animation-name](#propdef-animation-name title=animation-name "animation-name")
	whose values were not found earlier in the shorthand
	must be accepted for those properties rather than for
	[animation-name](#propdef-animation-name title=animation-name "animation-name"). Furthermore, when serializing, default values of other properties must be
	output in at least the cases necessary to distinguish an [animation-name](#propdef-animation-name title=animation-name "animation-name") that could
	be a value of another property, and may be output in additional cases.

	<div class=example>
		For example, a value parsed from [animation: 3s none backwards](#propdef-animation title=animation "animation")
		(where [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") is [none](#valuedef-none title=none "none")
		and [animation-name](#propdef-animation-name title=animation-name "animation-name") is <span class=css data-link-for=animation-name data-link-type=maybe title=backwards>backwards</span>)
		must not be serialized as [animation: 3s backwards](#propdef-animation title=animation "animation")
		(where [animation-fill-mode](#propdef-animation-fill-mode title=animation-fill-mode "animation-fill-mode") is [backwards](#valuedef-backwards title=backwards "backwards")
		and [animation-name](#propdef-animation-name title=animation-name "animation-name") is [none](#valuedef-none0 title=none "none")).
	</div>

## <span class=secno>5 </span><span class=content>
Animation Events</span>[](#events)

<p>	Several animation-related events are available through the DOM Event system. The start and
	end of an animation, and the end of each iteration of an animation, all generate DOM events.
	An element can have multiple properties being animated simultaneously. This can occur either
	with a single [animation-name](#propdef-animation-name title=animation-name "animation-name") value with keyframes containing multiple properties, or with
	multiple [animation-name](#propdef-animation-name title=animation-name "animation-name") values. For the purposes of events, each [animation-name](#propdef-animation-name title=animation-name "animation-name") specifies
	a single animation. Therefore an event will be generated for each [animation-name](#propdef-animation-name title=animation-name "animation-name") value and
	not necessarily for each property being animated.

<p>	Any animation for which a valid keyframe rule is defined will run
	and generate events; this includes animations with empty keyframe rules.

<p>	The time the animation has been running is sent with each event generated. This allows the event
	handler to determine the current iteration of a looping animation or the current position of an
	alternating animation. This time does not include any time the animation was in the [paused](#valuedef-paused title=paused "paused")
	play state.

### <span class=secno>5.1 </span><span class=content>
The `AnimationEvent` Interface</span>[](#interface-animationevent)

<p>	The <dfn data-dfn-type=dfn data-noexport="" id=animationevent>AnimationEvent[](#animationevent)</dfn> interface provides specific contextual information associated with
	Animation events.

#### <span class=secno>5.1.1 </span><span class=content>
IDL Definition</span>[](#interface-animationevent-idl)

	<pre class=idl>[Constructor(DOMString <dfn class=idl-code data-dfn-for=AnimationEvent/AnimationEvent() data-dfn-type=argument data-export="" data-global-name="AnimationEvent<interface>/AnimationEvent()<method>/type<argument>" id=dom-animationeventanimationevent-type>type[](#dom-animationeventanimationevent-type)</dfn>, optional [AnimationEventInit](#dictdef-animationeventinit title=animationeventinit "animationeventinit") <dfn class=idl-code data-dfn-for=AnimationEvent/AnimationEvent() data-dfn-type=argument data-export="" data-global-name="AnimationEvent<interface>/AnimationEvent()<method>/animationeventinitdict<argument>" id=dom-animationeventanimationevent-animationeventinitdict>animationEventInitDict[](#dom-animationeventanimationevent-animationeventinitdict)</dfn>)]
  interface <dfn class=idl-code data-dfn-type=interface data-export="" data-global-name="" id=dom-animationevent>AnimationEvent[](#dom-animationevent)</dfn> : [Event](http://dom.spec.whatwg.org/#event title=event "event") {
    readonly attribute DOMString <dfn class=idl-code data-dfn-for=AnimationEvent data-dfn-type=attribute data-export="" data-global-name="AnimationEvent<interface>/animationname<attribute>" id=dom-animationevent-animationname0>animationName[](#dom-animationevent-animationname0)</dfn>;
    readonly attribute float <dfn class=idl-code data-dfn-for=AnimationEvent data-dfn-type=attribute data-export="" data-global-name="AnimationEvent<interface>/elapsedtime<attribute>" id=dom-animationevent-elapsedtime0>elapsedTime[](#dom-animationevent-elapsedtime0)</dfn>;
    readonly attribute DOMString <dfn class=idl-code data-dfn-for=AnimationEvent data-dfn-type=attribute data-export="" data-global-name="AnimationEvent<interface>/pseudoelement<attribute>" id=dom-animationevent-pseudoelement0>pseudoElement[](#dom-animationevent-pseudoelement0)</dfn>;
  };
  dictionary <dfn class=idl-code data-dfn-type=dictionary data-export="" data-global-name="" id=dictdef-animationeventinit>AnimationEventInit[](#dictdef-animationeventinit)</dfn> : [EventInit](http://dom.spec.whatwg.org/#eventinit title=eventinit "eventinit") {
    DOMString <dfn class=idl-code data-dfn-for=AnimationEventInit data-dfn-type=dict-member data-export="" data-global-name="AnimationEventInit<dictionary>/animationname<dict-member>" id=dom-animationeventinit-animationname>animationName[](#dom-animationeventinit-animationname)</dfn> = "";
    float <dfn class=idl-code data-dfn-for=AnimationEventInit data-dfn-type=dict-member data-export="" data-global-name="AnimationEventInit<dictionary>/elapsedtime<dict-member>" id=dom-animationeventinit-elapsedtime>elapsedTime[](#dom-animationeventinit-elapsedtime)</dfn> = 0.0;
    DOMString <dfn class=idl-code data-dfn-for=AnimationEventInit data-dfn-type=dict-member data-export="" data-global-name="AnimationEventInit<dictionary>/pseudoelement<dict-member>" id=dom-animationeventinit-pseudoelement>pseudoElement[](#dom-animationeventinit-pseudoelement)</dfn> = "";
  };
</pre>

#### <span class=secno>5.1.2 </span><span class=content>
Attributes</span>[](#interface-animationevent-attributes)

	<dl data-dfn-for=animationevent data-dfn-type=attribute>
		<dt><dfn class=idl-code data-dfn-for=animationevent data-dfn-type=attribute data-export="" id=dom-animationevent-animationname>animationName[](#dom-animationevent-animationname)</dfn>, of type <a class=idl-code data-link-type=interface title=domstring>DOMString</a>, readonly
		<dd>
			The value of the [animation-name](#propdef-animation-name title=animation-name "animation-name") property of the animation that fired the event.
		<dt><dfn class=idl-code data-dfn-for=animationevent data-dfn-type=attribute data-export="" id=dom-animationevent-elapsedtime>elapsedTime[](#dom-animationevent-elapsedtime)</dfn>, of type float, readonly
		<dd>
			The amount of time the animation has been running, in seconds, when this event fired,
			excluding any time the animation was paused. For an <span class=css data-link-type=maybe title=animationstart>animationstart</span> event, the
			elapsedTime is zero unless there was a negative value for [animation-delay](#propdef-animation-delay title=animation-delay "animation-delay"), in which
			case the event will be fired with an elapsedTime of (-1 * delay).
		<dt><dfn class=idl-code data-dfn-for=animationevent data-dfn-type=attribute data-export="" id=dom-animationevent-pseudoelement>pseudoElement[](#dom-animationevent-pseudoelement)</dfn>, of type <a class=idl-code data-link-type=interface title=domstring>DOMString</a>, readonly
		<dd>
			The name (beginning with two colons) of the CSS pseudo-element on which the animation
			runs (in which case the target of the event is that pseudo-element’s corresponding
			element), or the empty string if the animation runs on an element (which means the
			target of the event is that element).
	</dl>

<p>	<dfn class=css-code data-dfn-type=function data-export="" id=funcdef-animationevent title=animationevent()>AnimationEvent(type, animationEventInitDict)[](#funcdef-animationevent)</dfn> is an [event constructor](http://dvcs.w3.org/hg/domcore/raw-file/tip/Overview.html#constructing-events).

### <span class=secno>5.2 </span><span class=content>
Types of `AnimationEvent`</span>[](#event-animationevent)

<p>	The different types of animation events that can occur are:

	<dl data-dfn-for=animationevent data-dfn-type=event>
		<dt><dfn class=idl-code data-dfn-for=animationevent data-dfn-type=event data-export="" id=dom-animationevent-animationstart>animationstart[](#dom-animationevent-animationstart)</dfn>
		<dd>
			The [animationstart](#dom-animationevent-animationstart title=animationstart "animationstart") event occurs at the start of the animation. 
			If there is an [animation-delay](#propdef-animation-delay title=animation-delay "animation-delay") then this event will fire once the delay 
			period has expired. A negative delay will cause the event to fire with 
			an elapsedTime equal to the absolute value of the delay; in this case the
			event will fire whether [animation-play-state](#propdef-animation-play-state title=animation-play-state "animation-play-state") is set to [running](#valuedef-running title=running "running") or [paused](#valuedef-paused title=paused "paused").

*   Bubbles: Yes
*   Cancelable: No
*   Context Info: animationName, pseudoElement

		<dt><dfn class=idl-code data-dfn-for=animationevent data-dfn-type=event data-export="" id=dom-animationevent-animationend>animationend[](#dom-animationevent-animationend)</dfn>
		<dd>
			The [animationend](#dom-animationevent-animationend title=animationend "animationend") event occurs when the animation finishes.

*   Bubbles: Yes
*   Cancelable: No
*   Context Info: animationName, elapsedTime, pseudoElement

		<dt><dfn class=idl-code data-dfn-for=animationevent data-dfn-type=event data-export="" id=dom-animationevent-animationiteration>animationiteration[](#dom-animationevent-animationiteration)</dfn>
		<dd>
			The [animationiteration](#dom-animationevent-animationiteration title=animationiteration "animationiteration") event occurs at the end of each iteration of an
			animation, except when an animationend event would fire at the same time.
			This means that this event does not occur for animations with an iteration
			count of one or less.

*   Bubbles: Yes
*   Cancelable: No
*   Context Info: animationName, elapsedTime, pseudoElement
	</dl>

## <span class=secno>6 </span><span class=content>
DOM Interfaces</span>[](#interface-dom)

<p>	CSS animations are exposed to the CSSOM through a pair of new interfaces describing the keyframes.

### <span class=secno>6.1 </span><span class=content>
The `CSSRule` Interface</span>[](#interface-cssrule)

<p>	The following two rule types are added to the [CSSRule](http://dev.w3.org/csswg/cssom-1/#cssrule title=cssrule "cssrule") interface. They provide
	identification for the new keyframe and keyframes rules.

#### <span class=secno>6.1.1 </span><span class=content>
IDL Definition</span>[](#interface-cssrule-idl)

	<pre class=idl>partial interface [CSSRule](http://dev.w3.org/csswg/cssom-1/#cssrule title=cssrule "cssrule") {
    const unsigned short <dfn class=idl-code data-dfn-for=CSSRule data-dfn-type=const data-export="" data-global-name="CSSRule<interface>/keyframes_rule<const>" id=dom-cssrule-keyframes_rule>KEYFRAMES_RULE[](#dom-cssrule-keyframes_rule)</dfn> = 7;
    const unsigned short <dfn class=idl-code data-dfn-for=CSSRule data-dfn-type=const data-export="" data-global-name="CSSRule<interface>/keyframe_rule<const>" id=dom-cssrule-keyframe_rule>KEYFRAME_RULE[](#dom-cssrule-keyframe_rule)</dfn> = 8;
};
</pre>

### <span class=secno>6.2 </span><span class=content>
The `CSSKeyframeRule` Interface</span>[](#interface-csskeyframerule)

<p>	The [CSSKeyframeRule](#dom-csskeyframerule title=csskeyframerule "csskeyframerule") interface represents the style rule for a single key.

#### <span class=secno>6.2.1 </span><span class=content>
IDL Definition</span>[](#interface-csskeyframerule-idl)

	<pre class=idl>interface <dfn class=idl-code data-dfn-type=interface data-export="" data-global-name="" id=dom-csskeyframerule>CSSKeyframeRule[](#dom-csskeyframerule)</dfn> : [CSSRule](http://dev.w3.org/csswg/cssom-1/#cssrule title=cssrule "cssrule") {
           attribute DOMString           <dfn class=idl-code data-dfn-for=CSSKeyframeRule data-dfn-type=attribute data-export="" data-global-name="CSSKeyframeRule<interface>/keytext<attribute>" id=dom-csskeyframerule-keytext>keyText[](#dom-csskeyframerule-keytext)</dfn>;
  readonly attribute [CSSStyleDeclaration](http://dev.w3.org/csswg/cssom-1/#cssstyledeclaration title=cssstyledeclaration "cssstyledeclaration") <dfn class=idl-code data-dfn-for=CSSKeyframeRule data-dfn-type=attribute data-export="" data-global-name="CSSKeyframeRule<interface>/style<attribute>" id=dom-csskeyframerule-style0>style[](#dom-csskeyframerule-style0)</dfn>;
};
</pre>

#### <span class=secno>6.2.2 </span><span class=content>
Attributes</span>[](#interface-csskeyframerule-attributes)

	<dl data-dfn-for=csskeyframerule data-dfn-type=attribute>

		<dt><dfn class=idl-code data-dfn-for=CSSKeyFramesRule data-dfn-type=attribute data-export="" id=dom-csskeyframesrule-keytext>keyText[](#dom-csskeyframesrule-keytext)</dfn>, of type <a class=idl-code data-link-type=interface title=domstring>DOMString</a>
		<dd>
			This attribute represents the keyframe selector as a comma-separated list of
			percentage values. The <span class=css data-link-type=maybe title=from>from</span> and <span class=css data-link-type=maybe title=to>to</span> keywords map to 0% and 100%,
			respectively.

<p>            If [keyText](#dom-csskeyframesrule-keytext title=keytext "keytext") is updated with an invalid keyframe selector, 
            a [SyntaxError](http://dom.spec.whatwg.org/#syntaxerror title=syntaxerror "syntaxerror") exception must be thrown.

		<dt><dfn class=idl-code data-dfn-for=csskeyframerule data-dfn-type=attribute data-export="" id=dom-csskeyframerule-style>style[](#dom-csskeyframerule-style)</dfn>, of type [CSSStyleDeclaration](http://dev.w3.org/csswg/cssom-1/#cssstyledeclaration title=cssstyledeclaration "cssstyledeclaration")
		<dd>
			This attribute represents the style associated with this keyframe.
	</dl>

### <span class=secno>6.3 </span><span class=content>
The `CSSKeyframesRule` Interface</span>[](#interface-csskeyframesrule)

<p>	The [CSSKeyframesRule](#dom-csskeyframesrule title=csskeyframesrule "csskeyframesrule") interface represents a complete set of keyframes for
	a single animation.

#### <span class=secno>6.3.1 </span><span class=content>
IDL Definition</span>[](#interface-csskeyframesrule-idl)

	<pre class=idl>interface <dfn class=idl-code data-dfn-type=interface data-export="" data-global-name="" id=dom-csskeyframesrule>CSSKeyframesRule[](#dom-csskeyframesrule)</dfn> : [CSSRule](http://dev.w3.org/csswg/cssom-1/#cssrule title=cssrule "cssrule") {
           attribute DOMString   <dfn class=idl-code data-dfn-for=CSSKeyframesRule data-dfn-type=attribute data-export="" data-global-name="CSSKeyframesRule<interface>/name<attribute>" id=dom-csskeyframesrule-name0>name[](#dom-csskeyframesrule-name0)</dfn>;
  readonly attribute [CSSRuleList](http://dev.w3.org/csswg/cssom-1/#cssrulelist title=cssrulelist "cssrulelist") <dfn class=idl-code data-dfn-for=CSSKeyframesRule data-dfn-type=attribute data-export="" data-global-name="CSSKeyframesRule<interface>/cssrules<attribute>" id=dom-csskeyframesrule-cssrules0>cssRules[](#dom-csskeyframesrule-cssrules0)</dfn>;

  void            <dfn class=idl-code data-dfn-for=CSSKeyframesRule data-dfn-type=method data-export="" data-global-name="CSSKeyframesRule<interface>/appendrule()<method>" id=dom-csskeyframesrule-appendrule0 title=appendRule()>appendRule[](#dom-csskeyframesrule-appendrule0)</dfn>(DOMString <dfn class=idl-code data-dfn-for=CSSKeyframesRule/appendRule() data-dfn-type=argument data-export="" data-global-name="CSSKeyframesRule<interface>/appendRule()<method>/rule<argument>" id=dom-csskeyframesruleappendrule-rule0>rule[](#dom-csskeyframesruleappendrule-rule0)</dfn>);
  void            <dfn class=idl-code data-dfn-for=CSSKeyframesRule data-dfn-type=method data-export="" data-global-name="CSSKeyframesRule<interface>/deleterule()<method>" id=dom-csskeyframesrule-deleterule0 title=deleteRule()>deleteRule[](#dom-csskeyframesrule-deleterule0)</dfn>(DOMString <dfn class=idl-code data-dfn-for=CSSKeyframesRule/deleteRule() data-dfn-type=argument data-export="" data-global-name="CSSKeyframesRule<interface>/deleteRule()<method>/key<argument>" id=dom-csskeyframesruledeleterule-key0>key[](#dom-csskeyframesruledeleterule-key0)</dfn>);
  [CSSKeyframeRule](#dom-csskeyframerule title=csskeyframerule "csskeyframerule") <dfn class=idl-code data-dfn-for=CSSKeyframesRule data-dfn-type=method data-export="" data-global-name="CSSKeyframesRule<interface>/findrule()<method>" id=dom-csskeyframesrule-findrule0 title=findRule()>findRule[](#dom-csskeyframesrule-findrule0)</dfn>(DOMString <dfn class=idl-code data-dfn-for=CSSKeyframesRule/findRule() data-dfn-type=argument data-export="" data-global-name="CSSKeyframesRule<interface>/findRule()<method>/key<argument>" id=dom-csskeyframesrulefindrule-key0>key[](#dom-csskeyframesrulefindrule-key0)</dfn>);
};
</pre>

#### <span class=secno>6.3.2 </span><span class=content>
Attributes</span>[](#interface-csskeyframesrule-attributes)

	<dl data-dfn-for=csskeyframesrule data-dfn-type=attribute>

		<dt><dfn class=idl-code data-dfn-for=csskeyframesrule data-dfn-type=attribute data-export="" id=dom-csskeyframesrule-name>name[](#dom-csskeyframesrule-name)</dfn>, of type <a class=idl-code data-link-type=interface title=domstring>DOMString</a>
		<dd>
			This attribute is the name of the keyframes, used by the [animation-name](#propdef-animation-name title=animation-name "animation-name") property.

		<dt><dfn class=idl-code data-dfn-for=csskeyframesrule data-dfn-type=attribute data-export="" id=dom-csskeyframesrule-cssrules>cssRules[](#dom-csskeyframesrule-cssrules)</dfn>, of type [CSSRuleList](http://dev.w3.org/csswg/cssom-1/#cssrulelist title=cssrulelist "cssrulelist")
		<dd>
			This attribute gives access to the keyframes in the list.
	</dl>

#### <span class=secno>6.3.3 </span><span class=content>
The `appendRule` method</span>[](#interface-csskeyframesrule-appendrule)

<p>	The <dfn class=idl-code data-dfn-for=CSSKeyFramesRule data-dfn-type=method data-export="" id=dom-csskeyframesrule-appendrule title=appendrule()>appendRule()[](#dom-csskeyframesrule-appendrule)</dfn> method appends the passed
	[CSSKeyframeRule](#dom-csskeyframerule title=csskeyframerule "csskeyframerule") into the list at the passed key.

<p>	Parameters:

	<dl>

		<dt><dfn class=idl-code data-dfn-for=CSSKeyFramesRule/appendRule() data-dfn-type=argument data-export="" id=dom-csskeyframesruleappendrule-rule>rule[](#dom-csskeyframesruleappendrule-rule)</dfn> of type <a class=idl-code data-link-type=interface title=domstring>DOMString</a>
		<dd>
			The rule to be appended, expressed in the same syntax as one entry in the
			[@keyframes](#at-ruledef-keyframes title=@keyframes "@keyframes") rule.
	</dl>

<p>	No Return Value

<p>	No Exceptions

#### <span class=secno>6.3.4 </span><span class=content>
The `deleteRule` method</span>[](#interface-csskeyframesrule-deleterule)

<p>	The <dfn class=idl-code data-dfn-for=CSSKeyFramesRule data-dfn-type=method data-export="" id=dom-csskeyframesrule-deleterule title=deleterule()>deleteRule()[](#dom-csskeyframesrule-deleterule)</dfn> deletes the [CSSKeyframeRule](#dom-csskeyframerule title=csskeyframerule "csskeyframerule")
	with the passed key. If a rule with this key does not exist, the method does nothing.

<p>	Parameters:

	<dl>

		<dt><dfn class=idl-code data-dfn-for=CSSKeyFramesRule/deleteRule() data-dfn-type=argument data-export="" id=dom-csskeyframesruledeleterule-key>key[](#dom-csskeyframesruledeleterule-key)</dfn> of type <a class=idl-code data-link-type=interface title=domstring>DOMString</a>
		<dd>
			The key which describes the rule to be deleted. A percentage value between 
            0% and 100%, or one of the keywords <span class=css data-link-type=maybe title=fro>fro</span> or <span class=css data-link-type=maybe title=to>to</span> which resolve to 0% and 
            100%, respectively.
	</dl>

<p>	No Return Value

<p>	No Exceptions

#### <span class=secno>6.3.5 </span><span class=content>
The `findRule` method</span>[](#interface-csskeyframesrule-findrule)

<p>	The <dfn class=idl-code data-dfn-for=CSSKeyFramesRule data-dfn-type=method data-export="" id=dom-csskeyframesrule-findrule title=findrule()>findRule()[](#dom-csskeyframesrule-findrule)</dfn> returns the rule with a key matching
	the passed key. If no such rule exists, a null value is returned.

<p>	Parameters:

	<dl>
		<dt><dfn class=idl-code data-dfn-for=CSSKeyFramesRule/findRule() data-dfn-type=argument data-export="" id=dom-csskeyframesrulefindrule-key>key[](#dom-csskeyframesrulefindrule-key)</dfn> of type <a class=idl-code data-link-type=interface title=domstring>DOMString</a>
		<dd>
			The key which describes the rule to be deleted. A percentage value between 
            0% and 100%, or one of the keywords <span class=css data-link-type=maybe title=fro>fro</span> or <span class=css data-link-type=maybe title=to>to</span> which resolve to 0% and 
            100%, respectively.	
    </dl>

<p>	Return Value:

	<dl>
		<dt>[CSSKeyframeRule](#dom-csskeyframerule title=csskeyframerule "csskeyframerule")
		<dd>
			The found rule.
	</dl>

<p>	No Exceptions

## <span class=secno>7 </span><span class=content>
Acknowledgements</span>[](#acknowledgements)

<p>	Thanks especially to the feedback from
	Tab Atkins,
	Carine Bournez,
	Christian Budde,
	Anne van Kesteren,
	Øyvind Stenhaug,
	Estelle Weyl,
	and all the rest of the www-style community.

## <span class=secno>8 </span><span class=content>
Working Group Resolutions that are pending editing</span>[](#wg-resolutions-pending)

<p>	_This section is informative and temporary._

<p>	The editors are currently behind on editing this spec. The following working group resolutions still
	need to be edited in:

<p>	

## <span class=content>Issues Index</span>[](#issues-index)
<div style="counter-reset: issue"><div class=issue>If similar to animation-duration:0s, also relates to whether
			animation events fire?
	[ ↵ ](#issue-ef1c522a)</div></div>