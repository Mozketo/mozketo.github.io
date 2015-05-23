---
layout: post
title: Determine if a String IsNumeric in C#
date: 2011-03-24 12:47
author: bclarkrobinson
comments: true
categories: [C#]
---
I've seen several implmentations of an IsNumeric() method in C# and all of them are woeful. Some examples involve casting and catch then catching exception, other (worse) examples recommend using Regular Expressions to check for only digits (no joke)!

Here's a nice and simple Extension Method to determine if a string IsNumeric().

<pre lang="csharp" colla="+">
/// <summary>
/// Determines whether the specified string is numeric.
/// </summary>
public static bool IsNumeric(this string input)
{
	double value;
	return double.TryParse(input, out value);
}
</pre>

I've even got some very simple Unit Tests to go with it (I understand I should not unit test .NET, but I wanted to ensure my implementation was fine).

<pre lang="csharp" colla="+">
[TestMethod]
public void IsNumeric_Numeric_String_Should_Evaluate_True()
{
	var expected = true;
	var text = "123456789.5";

	var actual = text.IsNumeric();

	Assert.AreEqual(expected, actual);
}

[TestMethod]
public void IsNumeric_Not_Numeric_String_Should_Evaluate_False()
{
	var expected = false;
	var text = "123456789rabbits";

	var actual = text.IsNumeric();

	Assert.AreEqual(expected, actual);
}
</pre>

Updated (5-June-2011): This method only tested whole numbers, decimals are now supported.
