---
layout: post
title: Core Animation CAConstraint Grid-Cell Layout
date: 2009-02-03 12:40
author: bclarkrobinson
comments: true
categories: [CAConstraint, Cocoa, Core Animation]
---
I really wanted to get a nice Grid Layout using CAConstraint, but I found the situation getting very complex very fast as each Cell needed to be able to reference the location of the last Cell and whether or not the Cell needed to drop down to a new Row, and at times needed to also reference the @"superlayer". There has to be a better way, right?

<a href="http://mozketo.com/wp-content/uploads/2009/02/caconstraint-gridlayout-sample.png"><img class="alignnone size-full wp-image-131" title="caconstraint-gridlayout-sample" src="http://mozketo.com/wp-content/uploads/2009/02/caconstraint-gridlayout-sample.png" alt="CAConstraint Grid-Cell Layout with Animations" width="351" height="172" /></a>

After a little digging around I was able to find an amazing Hot Chocolate <a href="http://devblog.brautaset.org/2008/10/01/calayer-grid-with-caconstraintlayoutmanager/">blog post</a> which showed a fantastic trick when laying out the Cells without having to reference the prior Cell. This approach takes advantage of knowing the Cell Row/Column position plus using the Scale: attribute of the CAConstraint on the @"superlayer" and it works like a charm.

<pre lang="objc" colla="+">[CAConstraint constraintWithAttribute: kCAConstraintWidth
relativeTo: @"superlayer"
attribute: kCAConstraintWidth
scale: 1.0 / columns
offset: 0]];</pre>

Want to change the number of Cells in the Grid? Modify these variables in this method:
<pre lang="objc" colla="+">
- (void)layoutCellsInGridLayer:(CALayer *)layer {
	int columns = 6;
	int rows = 6;...</pre>

Also to give the project a little bit of "wow" here's some random Y rotation on a Cell Layer:
<pre lang="objc">- (void)setupFlipAnimationOnLayer:(CALayer *)layer {
	float duration = (float)frandom(0.5, 5.0);

	CABasicAnimation* animation = [CABasicAnimation animationWithKeyPath:@"transform.rotation.y"];
	animation.fromValue = [NSNumber numberWithDouble:-1.0f * M_PI];
	animation.toValue = [NSNumber numberWithDouble:1.0f * M_PI];
	animation.duration = duration;
	animation.repeatCount = 1e100f;
	animation.beginTime = CACurrentMediaTime() + frandom(0.1, 30);
	animation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];

	[layer addAnimation:animation forKey:@"rotationY"];
}</pre>

To get the nice 3D perspective look the project needs the following code:
<pre lang="objc">- (void)setupPerspectiveWithX:(float)x andY:(float)y {
	CATransform3D transform = CATransform3DMakeRotation(x, 0, 1, 0);
	transform = CATransform3DRotate(transform, y, 1, 0, 0);
	float zDistance = -450;
	transform.m34 = 1.0 / -zDistance;
	gridLayer.sublayerTransform = transform;
}</pre>

And one last final note: take a look in <code>createCellInParentLayer:</code> and you will see <code>layer.contents</code>. If you can provide your own image the layer will automatically Fill (<code>layer.contentsGravity = kCAGravityResizeAspectFill;</code>) and Mask (<code>layer.masksToBounds = YES;</code>)  to make it fit.

You can grab the <a href='http://mozketo.com/wp-content/uploads/2009/02/caconstraint-grid-layout.zip'>code here</a> or download the <a href='http://mozketo.com/wp-content/uploads/2009/02/caconstraintgridlayoutapp.zip'>binary here</a> (requires Leopard).
