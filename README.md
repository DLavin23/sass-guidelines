##Syntax &amp; Formatting
Please note that Sass is the language and SCSS is simply a syntactically different version of the Sass language. The SCSS syntax is more representational of CSS, a language we all know and love, and is the syntax I prefer using in my projects. When I refer to or mention Sass in this document, it is implied that I am referring to the SCSS syntax of the Sass language.  Additionally, all code examples will be written in SCSS and will use the .scss files extention.

**Sass Syntax:**
```css
.your_new_class
	margin: 0
	padding: 0
```

**SCSS Syntax:**
```css
.your_new_class {
	margin: 0;
	padding: 0;
}
```

##Comments
Sass supports both visible and invisible comments.  Using <code>//</code> before any SCSS will place a comment in your SCSS code and will exclude it from the processed CSS.  Using standard <code>/* */</code> CSS comments will not only place a comment in your code, but will also include it in the processed CSS.  Personally, I recommend being very  generous with your comments, especially if you're working on large teams, as it will help other developers know what you're trying to accomplish and hopefully improve the debugging process.

```css
/* Im a visible comment and will show up in the processed CSS!*/
$base-hero-padding: 10px;
$main-color: blue;
$bg-color: #eee;

// Im invisible so you wont see me in the processed CSS!
.offer_wrapper {
	border: 1px solid black;
	margin: 0 auto;
	width: 500px;
}
```

##Variables
Sass allows you to store variables for later use, which eliminates the need to perform error prone search and replace functions every time you need to change a background color. Instead, simply change the value of the variable and let the Sass processor do the work for you! Variables are a extremely beneficial to your workflow and will go along way in terms of making changes faster and more efficiently.

**How to use:**
```css
//
// Variables begin with a $ and are declared just like properties
// They can have any value that is allowed for a CSS property such as colors, numbers, units, and text
//
$cox-bg-color: #cfdbe8   // light blue
$cox-primary-button-color: #236DA6   // bold blue
```

##Nesting
Indenting in Sass is NOT like indenting in CSS.  In Sass, nesting has meaning and uses the white space to declare parent child relationships.  This can be very powerful as it lets you inherit the parent selector so you don't have to repeat the beginning of selectors over and over again.  However, it's important to note that with great power, comes great responsibility!  Be careful not to fall in to the trap of nesting things unnecessarily or you will end up with bloated CSS and some pretty insane selector inheritance.  As a general rule of thumb, if you are tabbing a third time, pause and ask yourself, "is this declaration really that specific to this namespace or can it be abstracted?"

**Good Example of Nesting with SCSS:**
```css
.navbar {
  width: 80%;
  height: 25px;
  ul {
  	list-style-type: none;
  }
  li {
    float: left;
  }
  a {
  	font-weight: bold;
  }
}
```
**Output**
```css
.navbar {
	width: 80%;
	height: 25px;
}

.navbar ul {
	list-style-type: none;
}

.navbar li {
	float: left;
}

.navbar li a {
	font-weight: bold;
}
```

**Bad Example of Nesting with SCSS:**
While the developer probably had good intentions when writing this...
```css
product {
  .overview {
  	.header {
    	h2 {
    .content {
    	.promo {
      	.header{
        	div {
          	.inner{
            	a {
              	span{
              	}
              }
            }
          }
        }
      }
    }
  }
}
```

**Output**
...It actually produces the following CSS...YIKES!
```css
#product {}
#product .overview {}
#product .overview .header {}
#product .overview .header h2 {}
#product .overview .content {}
#product .overview .content .promo {}
#product .overview .content .promo .header {}
#product .overview .content .promo .header div {}
#product .overview .content .promo .header .inner {}
#product .overview .content .promo .header .inner a {}
#product .overview .content .promo .header .inner a span {}
```

###Declaration Best Practice
It is best practice to list your parent specific declarations directly under the class selector and then list the indented child selectors to enhance readability

```css
.fieldset {
    margin-bottom: 2em;
    p {
        margin-bottom: 0;
        line-height: 1.5em;
    }
    legend {
        float: left;
        width: 100%;
    }
}
```

###Nested Media Queries
Individual media queries can also be nested inside of a selector, changing property values based off a media condtion.

**For Example:**
```css
.container {
	width: 960px;
	@media screen and (max-width: 960px)
		width: 100%;
	}
}
```

**Output**
```css
.container {
	width: 960px;
}

@media screen and (max-width: 960px) {
	.container {
		width: 100%;
	}
}
```

###Selector Naming Conventions
This is all preference, just make sure you stay consistent and utilize the same patterns already being utilized in your project or on your team.  For example, <code>.my_awesome_class {}</code>.

##Mixins
Mixins are one of the most powerful features in Sass, as it lets you create logical chunks of code that can be reused again and again.  They are defined using the <code>@mixin</code> directive which takes a block of styles that can then be included in another selector using the <code>@include</code> directive. It is generally considered best practice to pass in arguments when using mixins.  Arguments are declared as a parenthesized, comma-seperated list of variables. Each of those variables is then assigned a value each time the mixin is used.

**Setting up the mixin:**
```css
@mixin the_box ($padding, $background, $border, $style, $color) {
	padding: $padding;
	background: $background;
	border: $border $style $color;
}
```

**Use the mixin via the <code>@include</code> directive:**
```css
.hero_area {
	@include the_box (10px, red, 1px, solid, black);
}
```

**Outputted CSS:**
```css
.hero_area {
	padding: 10px;
	background: red;
	border: 1px solid black;
}
```

##Selector Inheritance
In Sass, a selector can inherit attributes from any simple selector such as an element, class, or ID.  The <code>@extend</code> directive is used to indicate which selector to inherit from.  Unlike mixins, which apply contents into a selector, inheritance applies selectors to other selectors, which keeps stylesheets smaller while still providing many of the same advantages as mixins.
**Important** to note that <code>@extend</code> works by inserting the extending selector anywhere in the stylesheet that the extended selector appears. (<a href="https://gist.github.com/blackfalcon/4693231">Beware of the loop!</a>)

Example:
```css
.rounded {
	border-radius: 5px;
	border: 1px solid #ccc;
}

.hero-area {
	@extend .rounded;
	border-color: blue;
}

.hero-area-error {
	@extend .hero-area;
	border-color: red;
}
```

<code>@extend</code> is an awesome tool, however, if used incorrectly it can cause some issues.  A general rule of thumb and/or best practice is to never use extends inside of a mixin as it can create an amazing array of selectors in the outputted CSS.

###Placeholder Selectors
These selectors output nothing unless they are extended.

##Using Partials
Stylesheets can become pretty bloated when working on large applications/teams...and LFO is no exception!  Thankfully, Sass lets us break up styles into multiple stylesheets and import them without making numerous HTTP requests (i.e.<code>@import: url(...);</code>).  This allows us to break our code out into smaller, more managable chunks (yay for modularization!) without sacrificing optimization!

**Heres how it works:**

1. Sass uses a naming convention for files that are meant to be imported called <em>"partials"</em>, that begin with an underscore like so...<code>_navigation.scss</code>. When using this convention, Sass will not output a standalone CSS file; a partial is simply a resource file that other docs can consume and use.
2. Create a base stylesheet to store all the imported Sass files.
3. Use the <code>@import</code> directive to pull styles directly into a stylesheet. This will make available any variables or mixins defined in the <code>@imported</code> file.

```css
/* In style.css */

@import "header";
@import "layout";
@import "buttons";
@import "typography";
@import "footer";

/* the result of this is one stylesheet with all your styles
that can then be compressed and sent to the browser */
```

##Advanced Techniques
1. Functions and Operators
2. Color Manipulation
3. Calculations

