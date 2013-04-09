tween.js
========

#### Javascript Tweening Engine ####

Super simple, fast and easy to use tweening engine which incorporates optimised Robert Penner's equations. 
This Fork of the [original Repo](http://github.com/sole/tween.js) introduces orientation change listening, 
to improve the handling of device orientation changes during rotations
(this is especially useful for animations with end values based on the dimensions of the window).

### Usage ###

Download the [minified library](https://github.com/phil-brown/tween.js/raw/master/build/tween.min.js) and include it in your html.

```html
<script src="js/tween.min.js"></script>
```

The following code creates a Tween which will change the `x` attribute in a position variable, so that it goes from 1/8 of the screen width to 7/8 of the screen in 4 seconds. The anonymous function set up with an interval will update the screen so that we can see something happening:

```html
<script>

	init();
	animate();

	function init() {

		var output = document.createElement( 'div' );
		output.style.cssText = 'position: absolute; left: 50px; top: 300px; font-size: 100px';
		document.body.appendChild( output );
		
		start = { x: (window.innerHWidth || document.body.clientWidth)*0.125, y: (window.innerHeight || document.body.clientHeight)};
		end = { x: (window.innerHWidth || document.body.clientWidth)*0.875 };

		var tween = new TWEEN.Tween( { x: 50, y: 0 } )
			.to( { x: 400 }, 2000 )
			.easing( TWEEN.Easing.Elastic.InOut )
			.onUpdate( function () {

				output.innerHTML = 'x == ' + Math.round( this.x );
				output.style.left = this.x + 'px';

			} )
			.onOrientationChanged(function() {
			    end = { x: (window.innerHWidth || document.body.clientWidth)*0.875 };
			    var time = 4000 - arguments[1];
			    //ensure time value is not negative (should not occur)
			    if ( time < 0 ) {
			        time = 0;
			    }
			    this.to(end, time);
			} )
			.start();

	}

	function animate() {

		requestAnimationFrame( animate ); // js/RequestAnimationFrame.js needs to be included too.
		TWEEN.update();

	}

</script>
```

Note: this corresponds to the example [04_simplest.html](http://phil-brown.github.com/tween.js/examples/04_simplest.html) that you can find in the ```examples``` folder.

Have a look at that folder to discover more functionalities of the library!

Also, Jerome Etienne has written a [tutorial](http://learningthreejs.com/blog/2011/08/17/tweenjs-for-smooth-animation/) demonstrating how to use tween.js with three.js, and it's also great for understanding how tweens work!

### FAQ ###

**How do you set a tween to start after a while?**

Use the `delay()` method: `var t = new TWEEN.Tween({...}).delay(1000);`

**Is there a jQuery plug-in?**

No, we like to keep it simple and free of dependencies. Feel free to make one yourself, though! :-)

### Change log ###

2013 04 09 - **r10pb** 

* Forked to phil-brown, and added orientation change callbacks.

2013 03 03 - **r10** (5,342 KB, gzip: 2,010 KB)

* Added the ability to tween using relative values with ```to()``` ([endel](https://github.com/endel))

2013 02 04 - **r9** (5,224 KB, gzip: 1,959 KB)

* Use window.performance.now() if available for even smoother animations ([tdreyno](https://github.com/tdreyno), [mrdoob](https://github.com/mrdoob) and [sole](https://github.com/sole))
* Added tween.repeat() ([sole](https://github.com/sole))
* Improve example_01 performance ([mrdoob](https://github.com/mrdoob))
* Use CONTRIBUTING.md instead of having the section on README.md ([sole](https://github.com/sole))

2013 01 04 - **r8** (4,961 KB, gzip: 1,750 KB)

* New Date.now() shim by [roshambo](http://github.com/roshambo) makes the lib compatible with IE
* Fix for checking undefined `duration` ([deanm](http://github.com/deanm))
* Add unit tests ([sole](http://github.com/sole))
* Fixed non-existing properties sent in `to` and ending up as NaN in the target object ([sole](http://github.com/sole))
* Add missing example screenshot ([sole](http://github.com/sole))
* Add CONTRIBUTING section in README ([sole](http://github.com/sole))

2012 10 27 - **r7** (4,882 KB, gzip: 1,714 KB)

* Fixed start time of chained tweens when using custom timing. ([egraether](http://github.com/egraether))
* TWEEN.update() now returns a boolean (tweens pending or not). ([mrdoob](http://github.com/mrdoob))
* Added tween.onStart(). ([mrdoob](http://github.com/mrdoob))
* tween.chain() now accepts multiple tweens. ([mrdoob](http://github.com/mrdoob))


2012 04 10 - **r6** (4,707 KB, gzip: 1,630 KB)

* Returning instance also in `.chain()`. ([mrdoob](http://github.com/mrdoob))
* Refactoring and code clean up. ([egraether](http://github.com/egraether))
* Simplified easing formulas. ([infusion](http://github.com/infusion))
* Added support to arrays in `.to()` using linear, catmull-rom or bezier `.interpolation()`. ([egraether](http://github.com/egraether))
* Removed autostart/stop. ([mrdoob](http://github.com/mrdoob))
* Renamed `EaseNone`, `EaseIn`, `EaseOut` ane `EaseInOut`, to `None`, `In`, `Out` and `InOut`. ([mrdoob](http://github.com/mrdoob))
* Made `.to()` values dynamic. ([egraether](http://github.com/egraether) and [jeromeetienne](http://github.com/jeromeetienne))


2011 10 15 - **r5** (4,733 KB, gzip: 1,379 KB)

* Add autostart/stop functionalities ([jocafa](http://github.com/jocafa) and [sole](http://github.com/sole))
* Add 07_autostart example demonstrating the new functionalities ([sole](http://github.com/sole))


2011 10 15 - **r4**

* Use ``Date.now()`` instead of ``new Date.getTime()`` as it's faster ([mrdoob](http://github.com/mrdoob))


2011 09 30 - **r3**

* Added new ``time`` parameter to TWEEN.update, in order to allow synchronizing the tweens to an external timeline ([lechecacharro](http://github.com/lechecacharro))
* Added example to demonstrate the new synchronizing feature. ([sole](http://github.com/sole))


2011 06 18 - **r2**

* Added new utility methods getAll and removeAll for getting and removing all tweens ([Paul Lewis](http://github.com/paullewis))


2011 05 18 - **r1**

* Started using revision numbers in the build file
* Consider this kind of an stable revision :-)
