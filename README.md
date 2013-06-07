EmberCrossfilter
================

Both Ember DS and native JavaScript filtering methods are slow in comparison to Crossfilter. However, Crossfilter is not the easiest to get started with, and people starting out with Crossfilter find themselves in a pickle. That's why I've created a facade for working with Crossfilter with Ember. If you wish to go with your own implementation, then `EmberCrossfilter` also serves as a nice reference, and an example of a good implementation.

Out of the box, EmberCrossfilter provides:

* Create simple filters, such as ranges, custom callbacks, exact matches;
* Create more complicated boolean filters, such as OR/AND (`filterInArray`);
* Ability to determine which filters are active;
* Find the highest/lowest values quickly (`top`/`bottom`);
* Naturally sort an array using the `sort` object, and modify using the `sortContent` method;
* Ability to extend `EmberCrossfilter` by accessing the `_crossfilter` property;

Methods
-------------

`EmberCrossfilter` provides a simple interface with a minimal footprint.

It exposes the following public methods:

* `isActiveFilter(key, filter)` &ndash; determines whether a filter is currently active, and if a value is specified, whether the value equals that;
* `addFilter(key, value)` &ndash; add a new filter to filter against;
* `removeFilter(key, value)` &ndash; remove a filter that's already been applied using `addFilter`;
* `clearAllFilters` &ndash; clears all of the applied filters;
* `sortContent(property, isAscending)` &ndash; filters the content based on a property from the model;
* `top(property, count)` &ndash; a helper method for finding the highest value of `property`;
* `bottom(property, count)` &ndash; same as above, but the lowest value;

Timing
-------------

As Crossfilter is known to be extremely fast in comparison to both Ember DS and native JavaScript filtering/sorting, `EmberCrossfilter` likes to prove it &ndash; to see the timing outputs in your console, simply set `allowDebugging` to `true` in your controller that implements the `EmberCrossfilter` mixin, and you'll see how long various operations took in milliseconds.


Sorting
-------------

To set the default sorting direction for when `EmberCrossfilter` is initialised, you can supply a `sort` object in your controller.

The following will sort descending by the `name` property:

	sort: { sortProperty: 'name', isAscending: false }
	
If you'd like to trigger sorting updates from your controllers/views, then you can invoke the `sortContent` method with the property, and the direction (ascending/descending), and `EmberCrossfilter` will update the `sort` object for you to reflect the changes.

The following will change the sorting to sort ascending by the `age` property:

	sortContent('age', true);


More Complicated Stuff: Crossfilter's Missing Child
-------------

When using Crossfilter, it became apparent that comparing two arrays was naturally slow in Crossfilter, such as when you had `[1, 2, 3]` and you wanted only models with those values set. `EmberCrossfilter` rectifies that issue by offering a bitwise solution (`filterInArray`) which does all the hard-work for you. It can be configured to use OR/AND.

**AND (`boolean: 'and'`)**

If you've set `['Italy', 'Russia']` then a model must have BOTH of these values to be considered valid.

* Valid: `['Italy', 'Russia', 'Spain']`
* Invalid: `['Italy', 'Brazil', 'India']`

**OR (`boolean: 'or'`)**

In this case if you've set `['Italy', 'Russia']`, then a model can have either or both of these to be considered valid.

* Valid: `['Italy', 'Haiti']`
* Invalid: `['Portugal', 'Latvia']`