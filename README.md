#Bass grid

Flexible and semantic SASS based grid system to create beautiful responsive sites that will work on any device.

##Main features

* SASS based grid
* Fully responsive
* Fixed breakpoints for continuity (customisable and overridable)
* Custom grid size (total cols)
* Custom col widths as a ratio of grid size
* Choose mobile OR desktop first way of working
* Multiple custom gutters
* Works to IE7
* Allows IE specific styles in main SASS files for ease of readability
* JavaScript free
* All breakpoints and padding sizes calculated in ems
* Use both inline-block and float col methods - you don't have to choose
* Push and pull columns

##File structure

**grid.html** - documentation and grid example

**_functions.scss** - sass that is required but are common elements that may already be in your project (repetition not needed - just ensure they are there!)

**_grid-settings.scss** - REQUIRED - customisable settings for the grid

**_colspans-responsive.scss** - REQUIRED - grid structure - does not need to be touched

**_grid-example.scss** - styling for grid example and grid.html (NOT NEEDED)

##Running SASS

<div class="code"><pre><code>cd **to assets folder**
sass --watch sass:css --style compressed</code></pre></div>

##Basic grid

The basic grid is made up of wrapping containers called 'rows' that each contain a variable number of columns.

The Bass Grid does not require specific classes but rather uses a mix of mixins and placeholders that can be applied to custom classes as needed.

##How does the grid work?

The grid is based on paddings and margins. Each column has padding parameters that determine the gutter width (fully customisable). These should be set to 50% of the required gutter width/height between columns as the paddings do not collapse against each other. So if 2 columns each had a padding of 1em the total gutter width would be 2em.

The row is then given a negative margin of the same value as the gutter to ensure that the row starts at the expected '0' position and does not have extra spacing on any side of it as a result of the gutter. The position of the row is in no way affected by the gutter of the contained columns.

##Rows

To add a new row, the following code needs to be added to a wrapping element that will contain the grid columns and then customised to suit the purpose:

<div class="code"><pre><code>@include row($vertical-spacing, $horizontal-spacing);</code></pre></div>

Relace $vertical-spacing and $horizontal-spacing with custom values or variables that can be defined within the SASS partial _grid-settings.scss. The values for these should be in ems.

**NOTE:** Ensure the spacing for the row is IDENTICAL to that used in the contained columns. This will ensure that the correct negative margin is applied to the wrapper and that the columns all align to the edge of the container correctly.

##Colummn widths

Column widths are assigned as as ratio of the $grid-columns variable that can be set in _grid-settings.scss (default value: 12).

Breakpoints are predefined (see <a href="#breakpoints">Breakpoints and override</a> for more details) with 'Hippo' being the column sizing to use at desktop, 'Panda' at the next lowest breakpoint and 'Mouse' for the smallest, typically for smaller mobile devices.

The ratios for each breakpoint are defined using parameters within the column mixin as explained in the next section.

##Creating columns</h2>

Once the container row has been created, add divs with the following sass mixin applied in the styles to act as columns:

<div class="code"><pre><code>@include col($layout, $vertical-spacing, $horizontal-spacing, $hippo-cols, $panda-cols, $mouse-cols);</code></pre></div>

Customise the above code as follows:

* **$layout** - Replace this value with one of following:

	* **float** - columns to float left
	* **inline** - columns to display inline-block.
	* For more info see <a href="#inline-or-float">'choosing inline-block or float'</a>
	
* **$vertical-spacing, $horizontal-spacing** - Replace these values with custom values or variables that can be defined within the SASS partial _grid-settings.scss.
	* **NOTE:** Ensure the spacing for the row is IDENTICAL to that used on the row. This will ensure that the correct negative margin is applied to the wrapper and that the columns all align to the edge of the container correctly.

* **$hippo-cols** - Replace this with the column width for hippo screen sizes
	* Ensure this is declared as a number smaller than or equal to grid cols (as set in settings.scss)
	* The format of this parameter should be a simple number not in a string format or the word null (not in string)
	* If this is defined as null the col will be 100% at hippo size
* **$panda-cols** - Replace this with the column width for panda screen sizes
	* Ensure this is declared as a number smaller than or equal to grid cols (as set in settings.scss)
	* The format of this parameter should be a simple number not in a string format or the word null (not in string)
	* If this is defined as null the col will default to hippo size if exists
* **$mouse-cols** - Replace this with the column width for mouse screen sizes
	* Ensure this is declared as a number smaller than or equal to grid cols (as set in settings.scss)
	* The format of this parameter should be a simple number not in a string format or the word null (not in string)
	* If this is defined as null the col will be 100% at mouse size
	
Example usage:

<div class="code"><pre><code>@include col(float, $small-gutter, $small-gutter, 6, 6, null);</code></pre></div>
				
##Custom col widths:

This should be avoided if possible to maintain constistancy but if necessary the above breakpoints and column sizes at those breakpoints can be overriden as follows:

<div class="code"><pre><code>@include respond(1100) {
	width: 100% / $grid-columns * 1;
}</code></pre></div>

The '1' or chosen number should be used in the same way as the number following hippo/panda/mouse.

For example a column with this breakpoint will be able to fit 12 columns on 1 row using the default grid size.

##Clear floated columns

If float is the chosen method of positioning the grid columns, you may need to clear floats at different screen sizes to ensure that the grid clears correctly.

Each row will clear correctly before new content is started, but within that if columns need to be cleared at specific breakpoints (e.g. hippo, panda or mouse ONLY), add clearing divs after the necessary columns with the corresponding mixin applied:

<div class="code"><pre><code>@include clearfix(mouse);
@include clearfix(panda);
@include clearfix(hippo);</code></pre></div>


##Mobile or desktop first

By default this grid is designed to be developed using a mobile first methodology. This means that with NO media queries or breakpoints declared the default styles are for the smallest device size. Following this the next largest breakpoint (panda) is applied on top and then hippo for the largest devices.

Mobile first in IE and non-responsive browsers:</strong> Browsers such as IE8 do not support media queries and so traditionally mobile first development has relied on javascript to ensure that the site does not break and show only mobile styles on these devices at desktop. This grid uses a SASS function to determine if the browser is media query compatible and if it is not it will strip out the media queries and ensure that the latest defines styles (the desktop/hippo styles) override the mobile ones. This does mean it is very important that if mobile first methodology is being used that it is used consistantly throughout the site and good practise is used with regard defining the smallest device styles first and the desktop styles last.

###Choosing desktop first:

If desktop first methodology is preferred this can be used instead by chaning the value of the $mobile-first variable in _grid-settings.scss to false. This will then mean that media queries will go back to being ignored in IE and therefore only the default styles outside of media queries will be used as per normal.

**NOTE:** Please choose EITHER mobile or desktop first and do not interchange between the 2 styles of writing code.

##<a name="breakpoints">Breakpoints and override</a>

**Default breakpoints: ** Default breakpoints are declared in _grid-settings.scss for the hippo and panda sizes. Mouse is defaulted as anything below the panda breakpoint:

<div class="code"><pre><code>$panda: 480;
$hippo: 1024;</code></pre></div>

Ensure that when declaring the breakpoints above no 'px' or 'em' terms are added.The number should represent the px size for the breakpoint and this will be converted to ems by sass.

**Using the breakpoints:** To make use of the above breakpoints, do not use @media as standard as this will be implemented by the SASS function as necessary depending on if mobile/desktop first methodology is being used and the user's browser. Instead use the breakpoints using @include respond($panda) {} customised to the required breakpoint.

###Mobile first

<div class="code"><pre><code>//default (no media query) = mobile/mouse styles
@include respond($panda) { } //tablet/panda styles
@include respond($hippo) { } //largest/desktop/hippo styles</code></pre></div>

###Desktop first

<div class="code"><pre><code>//default (no media query) = largest/desktop/hippo styles
@include respond($hippo) { } //lower than hippo styles (i.e. tablet/panda styles)
@include respond($panda) { } //lower than panda styles (i.e. mobile/mouse styles)</code></pre></div>

###Custom breakpoints

This should be avoided if possible to maintain constistancy but if necessary the above breakpoints can be overrided as follows:

<div class="code"><pre><code>@include respond(1100) { }</code></pre></div>

The number following respond should represent the px size for the breakpoint. This will be converted to ems by sass.

##Breakpoint specific changes

If a change for a specific breakpoint is required which then has no effect on any of the other screensizes, the following code can be used to implement this. This will still work cross browser as the media queries will be stripped for desktop on older IE browsers.

<div class="code"><pre><code>//MOUSE/MOBILE ONLY
//@include respond-specific(mouse) {}
//PANDA/TABLET ONLY
//@include respond-specific(panda) {}
//HIPPO/DESKTOP ONLY
//@include respond-specific(hippo) {}</code></pre></div>
				
##<a name="breakpoints">Older IE specific changes</a>

Rather than having to have a seperate stylsheet with repeated classes for IE, a mixin has been created to allow styles to be made within the initial class declaration that will impact only older IE browsers. This will make the code a lot clearer and easier to navigate.

<div class="code"><pre><code>@include old-ie { }</code></pre></div>

##<a name="inline-or-float">Choosing inline block or float</a>

The choice of using inline-block or float is left entirely to the discretion of the user and can be easily implemented by changing the display property on the cols to either inline or float.

The benefit of inline-block is that these columns don't need to be cleared, so no clearfix solutions or overflow: hidden are required. Of course other elements can't flow around them and you can't force an element below an inline-block by clearing it. Inline-block elements are also far more flexible than floats, allowing ease of changing horizontal positon and vertical alignment. They do however pick up HTML whitespace and so use caution when using the columns using this method to ensure that there is no whitespace between the columns as this will cause the grid to break to a new line earlier than expected. This can be done by setting parent font-size to 0 or removing the whitespace with comments or lack of spacing. If the site is required to work in IE6 or IE7 use float instead as inline-block is not supported in these earlier browsers.

Floated elements will sit adjacent to each other, regardless of the html whitespace between them. They are also IE6 and IE7 compatible. They do however require clearfix solutions to be added where the columns are flowing underneath each other to ensure they clear correctly.
