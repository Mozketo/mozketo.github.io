---
layout: post
title: Barebones CAEmitterLayer aka Particle Effects
date: 2010-02-24 22:18
author: bclarkrobinson
comments: true
categories: [Apple, CAEmitterLayer, Core Animation]
---
<blockquote>CAEmitterLayer: use to create particle effects. Each particle is an instance of CAEmitterCell. (10.6) - <a href="http://twitter.com/cocoadevcentral/status/3607680055">cocoadevcentral</a></blockquote>

How could you not be intrigued? Particle Effects! Well I've always been interested in Particle Generators, and there's one right here in Snow Leopard. How exciting. /claps.

I recently took a look at the Fire and Fireworks sample code that Apple and was duly impressed by what Core Animation provides for so little cost both code-wise and CPU resources. So I decided to write the simplest - barebones - <a href="http://developer.apple.com/mac/library/documentation/GraphicsImaging/Reference/CAEmitterLayer_class/Reference/Reference.html">CAEmitterLayer</a> code where I was only dependant on the NSView it draws on.

There's no fancy-pants stuff going on here, just a raw, simple, single particle effect. But it gives you a great perspective on how it works and the minimal code required to have a great particle animation.

<a href="/wp-content/uploads/2010/02/particle_01_01.png"><img src="/wp-content/uploads/2010/02/particle_01_01.png" alt="Screenshot of this project" title="particle_01_01" width="531" height="519" class="alignnone size-full wp-image-362" /></a>

If you're starting afresh you're going to need to tell your project that we want to use Core Animation so:

<ul>
        <li>Right click the Frameworks > Linked Frameworks on the left,</li>
	<li>Add > Existing Frameworks...</li>
	<li>Scroll to QuartzCore.framework > Add,</li>
	<li>And make sure your ParticleController header file contains <code>#import &lt;QuartzCore/QuartzCore.h&gt;</code>.</li>
</ul>

Looking at CAEmitterLayer briefly (and ignoring setting up Interface Builder & the Nib) in our header file we simply need a CAEmitterLayer:

<pre lang="objc" colla="+">
IBOutlet NSView *view;
CALayer *rootLayer;
CAEmitterLayer *emitter;
</pre>

Where as the implementation file is going to need a whole lot of data for the Emitter plus we need to create a CAEmitterCell for the particle itself.

<pre lang="objc" colla="+">
//Create the emitter layer
emitter = [CAEmitterLayer layer];
emitter.emitterPosition = CGPointMake(CGRectGetMidX(rootLayer.bounds), CGRectGetMidY(rootLayer.bounds));
emitter.emitterMode = kCAEmitterLayerOutline;
emitter.emitterShape = kCAEmitterLayerCircle;
emitter.renderMode = kCAEmitterLayerAdditive;
emitter.emitterSize = CGSizeMake(50 * multiplier, 0);
	
//Create the emitter cell
CAEmitterCell* particle = [CAEmitterCell emitterCell];
particle.emissionLongitude = M_PI;
particle.birthRate = multiplier * 1000.0;
particle.lifetime = multiplier;
particle.lifetimeRange = multiplier * 0.35;
particle.velocity = 180;
particle.velocityRange = 130;
particle.emissionRange = 1.1;
particle.scaleSpeed = 0.3;
CGColorRef color = CGColorCreateGenericRGB(0.3, 0.4, 0.9, 0.10);
particle.color = color;
CGColorRelease(color);
particle.contents = (id) [self CGImageNamed:@"spark.png"];
</pre>

For the complete implementation file or the complete project please <a href="http://bitbucket.org/Mozketo/barebones-ca-particles-01">feel free to grab the source code from the bitbucket project</a>.
