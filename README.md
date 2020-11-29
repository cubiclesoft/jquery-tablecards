jQuery Responsive Table Cards
=============================

This jQuery plugin generates cards to convert wide tables into one or two column tables on smaller screen sizes by injecting a new column into each row of the table and using CSS to switch between table and card modes.

When generating cards, each row (including thead and tfoot) is processed using a string-based template.  This makes for highly efficient delivery of naturally tabular data to all devices but only generate the extra column on smaller displays.

Combined with [TableBodyScroll](https://github.com/cubiclesoft/jquery-tablebodyscroll), traditional tables can be displayed on all devices in a neat, compact format.

[![Donate](https://cubiclesoft.com/res/donate-shield.png)](https://cubiclesoft.com/donate/) [![Discord](https://img.shields.io/discord/777282089980526602?label=chat&logo=discord)](https://cubiclesoft.com/product-support/github/)

Usage
-----

```html
<link rel="stylesheet" href="jquery.tablecards.css" type="text/css" media="all" />
<script type="text/javascript" src="jquery.tablecards.js"></script>
<script type="text/javascript">
$(function() {
	var options = {
		width: 800,
		extracols: [1],
		head: "Info/Options",
		body: "<b>#%2 - %3</b><br>%5<br>%6"
	};

	$('#mytable').TableCards(options);

	// Some optional custom event handlers.
	$('#mytable').on('tablecards:mode', function() {
		$(this).trigger('table:columnschanged');
	}).on('table:datachanged', function() {
		$(this).trigger('tablecards:datachanged');
	}).on('table:resized', function() {
		$(this).trigger('tablecards:resize');
	});

	// A custom resize/visibility event handler ('child:visibility' is a custom FlexForms Extras handler emitted by an Accordion callback).
	var resizefunc = function() {
		var obj = $('#mytable');

		if (!obj.length)  $(window).off('resize', resizefunc).off('child:visibility', resizefunc);
		else  obj.trigger('tablecards:resize');
	};

	$(window).resize(resizefunc).on('child:visibility', resizefunc);
});
</script>
```

There are several live examples under "Add Entry" in the [Admin Pack Demo](http://barebonescms.com/demos/admin_pack/admin.php).

Limitations
-----------

Due to the way Responsive Table Cards works, the 'id' attribute may not be used in any part of the table's contents.  Use 'class' and 'data-' instead.  Doing so can be more semantic that way anyway.

Attached events on elements are also lost during card creation.  This is unavoidable.  Listen for the 'tablecards:mode' event on the table to trigger the first time this plugin switches to card mode.

Options
-------

The following options may be passed to TableCards:

* width - An integer to set the threshold after which to switch to and from card mode OR null to automatically determine the width later if the table exceeds its parent's width (Default is null).
* extracols - An array containing the columns to not hide in the header (Default is []).
* tokenstart - A string containing the prefix to use for the templates to reference column numbers (Default is '%').
* head - A string or an array of strings containing the templates to use in rotation for each row of the 'thead's (Default is '&nbsp;').
* body - A string or an array of strings containing the templates to use in rotation for each row of the 'tbody's (Default is '&nbsp;').
* foot - A string or an array of strings containing the templates to use in rotation for each row of the 'tfoot's (Default is '&nbsp;').
* postinit - A callback function to run after each TableCards initialization is run (Default is null).

To destroy the instance and restore the table to its initial state, simply call:  `$('#mytable').TableCards('destroy');`

Events
------

The following custom events may be listened for:

* tablecards:mode - Notifies after switching the mode.  An optional parameter is passed containing a string with the new mode - either 'card' or 'table'.

The following custom events may be manually triggered:

* tablecards:resize - Notifies TableCards that the table has been resized.
* tablecards:datachanged - Notifies TableCards that the data in the table has changed and to reset any width calculations at the first available opportunity.
