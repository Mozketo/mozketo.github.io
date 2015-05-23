---
layout: post
title: Resize UIView/UITableView when Keyboard displays
date: 2009-06-02 10:33
author: bclarkrobinson
comments: true
categories: [Apple, Cocoa]
---
I was having a hell of a time trying to get my UITableView to resize itself after the iPhone keyboard displayed itself. After being just a little surprised that the iPhone doesn't resize the underlying UIView for free I figured it was up to me to do resize.

Firstly add a few variables and method declares into your ViewController.h header file:

<pre lang="objc" colla="+">
Boolean keyboardIsShowing;
CGRect keyboardBounds;
- (void)resizeViewControllerToFitScreen;
</pre>

Now we need to register for the UIKeyboardWillShowNotification and the UIKeyboardWillHideNotification:

<pre lang="objc" colla="+">
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillShow:) name:UIKeyboardWillShowNotification object:nil];
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillHide:) name:UIKeyboardWillHideNotification object:nil];
</pre>

And these notifications need somewhere to go:

<pre lang="objc" colla="+">
#pragma mark -
#pragma mark Keyboard Handling

- (void)keyboardWillShow:(NSNotification *)notification {
	NSDictionary *userInfo = [notification userInfo];
	NSValue *keyboardBoundsValue = [userInfo objectForKey:UIKeyboardBoundsUserInfoKey];
	[keyboardBoundsValue getValue:&keyboardBounds];
	keyboardIsShowing = YES;
	[self resizeViewControllerToFitScreen];
}

- (void)keyboardWillHide:(NSNotification *)note {
	keyboardIsShowing = NO;
	keyboardBounds = CGRectMake(0, 0, 0, 0);
	[self resizeViewControllerToFitScreen];
}
</pre>

And now add the magic method to resize the view:

<pre lang="objc" colla="+">
- (void)resizeViewControllerToFitScreen {
	// Needs adjustment for portrait orientation!
	CGRect applicationFrame = [[UIScreen mainScreen] applicationFrame];
	CGRect frame = self.view.frame;
	frame.size.height = applicationFrame.size.height;
	
	if (keyboardIsShowing)
		frame.size.height -= keyboardBounds.size.height;

	[UIView beginAnimations:nil context:NULL];
	[UIView setAnimationBeginsFromCurrentState:YES];
	[UIView setAnimationDuration:0.3f];
	self.view.frame = frame;
	[UIView commitAnimations];
}
</pre>

And super importantly de-register the ViewController from those notifications.

<pre lang="objc" colla="+">
- (void)viewWillDisappear:(BOOL)animated {
	[[NSNotificationCenter defaultCenter] removeObserver:self name:UIKeyboardWillShowNotification object:nil];
	[[NSNotificationCenter defaultCenter] removeObserver:self name:UIKeyboardWillHideNotification object:nil];
}
</pre>

See how this all comes together?

1. We register for Notifications when the Keyboard shows/hides,
2. We react when the keyboard is shown/hidden,
3. The resizeViewControllerToFitScreen method handles our resize, including animating the underlying view so it looks pretty.

There's a few caveats:

1. I've not tested on Landscape mode, I'm pretty sure this will fail.
2. UIKeyboardWillShowNotification can get fired every time you enter a textbox (as I've only one textbox it's not a problem for me). So you might need to look at using ...TextDidBeginEditing/...TextDidEndEditing or maintaining state differently so that the view isn't jumping all over the place.
