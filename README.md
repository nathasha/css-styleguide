# CSS Coding Standards

## General
We use [SASS](http://sass-lang.com/) for our CSS preprocessor. 

Fully document your css. Treat it like you would JavaScript. If you are adding properties, to try and deal with an issue or you are doing tricky work with pseudo elements etc, make sure you are documenting.

Stay consistent with your code. If you feel the standards need expansion or changes, bring them up with the team for discussion. It's not as important what we do but that we all do.

:question: => [:email:](mailto:frontend@mediatemple.net) or [Slack](https://godaddy.slack.com/archives/mt-front-end).  

## Terminology
### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

<pre>
.listing {
  font-size: 18px;
  line-height: 1.2;
}
</pre>

### Selectors
In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

<pre>
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
</pre>

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

<pre>
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
</pre>


## Rules
These are specific and strict rules to be adhered to when writing CSS.


### Formatting
Use hard tabs for indentation.

### Selectors
IDs should be reserved strictly for JavaScript hooks.
While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable. ( For more on this subject, read [hacks-for-dealing-with-specificity](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/)).

### Javascript Hooks
JavaScript-specific selectors should prefixed with **js-**


```
/*
* js prefix: 'js-request-to-book
*/
<button class="btn js-request-to-book">Request to Book</button>

```

You can also bind to selectors that represent a state or a condition (is-hidden , has-options).

```
/*
*  CSS: 'is-disabled'
*/

<button class='btn is-disabled'>Click</button>

```

```
/*
* In Javascript, you can select the state component
*/
document.queryselectorall('.is-disabled');

```


**Avoid !important** in your CSS rules. There are very limited cases in which the use of !important in a rule acceptable. For our purposes the only acceptable use is in utility classes. Since the whole point of the utility call is to apply a style like ''float: left'' it is acceptable to use here.


### Avoid class names with type selectors
```
/* Bad */
button.btn { }  
 
/* Good */
.btn {}

```

### Ordering of property declarations
1. **Property declarations**

	List all standard property declarations, anything that isn't an @include or a nested selector.
	
	<pre>
	.btn-green {
	  background: green;
	  font-weight: bold;
	  // ...
	}
	</pre>

2. **@include declarations**

	Grouping @includes at the end makes it easier to read the entire selector.
	
	<pre>
	.btn-green {
	  background: green;
	  font-weight: bold;
	  @include transition(background 0.5s ease);
	  // ...
	}
	</pre>

3. **Nested selectors**

	Nested selectors, if necessary, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.
	
	<pre>
	.btn {
	  background: green;
	  font-weight: bold;
	  @include transition(background 0.5s ease);
	
	  .icon {
	    margin-right: 10px;
	  }
	}
	</pre>

### Variables
Use variables for recurring values.

Variables should be **UPPER_CASE** and use **underscores** to separate words.

Variables that should not be used as values of properties, but should be assigned to other variables, should use the start with an underscore.

<pre>
$_PINK: DeepPink;
 
$BTN_BG: $_PINK;
 
$BASE_FONT_SIZE: 16px;
 
$GUTTER: 1.5em;
</pre>

### Nest and Name Your Media Queries
Why?

1. You don't have to re-write the selector somewhere else, which can be error prone.
2. The rules that you are overriding are very clear and obvious, which is usually not the case when they are at the bottom of your CSS or in a different file.

<pre>
.btn {
    float: right;
    width: 33.33%;
 
    @media (max-width: 650px) {
        width 100%;
    }
}
</pre>


## Code Pattern

We use [BEMIT](http://csswizardry.com/2015/08/bemit-taking-the-bem-naming-convention-a-step-further/) as our CSS coding pattern. BEMIT is a slightly extended version of [BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) (Block-Element-Modifier).

**Block**  represents the higher level of an abstraction or component.

**Element** represents a descendent of *.block* that helps form.block as a whole.

**Modifier** represents a different state or version of *.block*

### Naming Convention

<pre>

// The top-level ‘Block’ of a component.
.block {}

// An ‘Element’ that is a descendent of the Block.
.block__title {}

// A ‘Modifier’ of the Block.
.block--large {}

</pre>

Names should be all lower-case and words should be separated by hyphens.

<pre>

/* Bad */
---------
// Block
.blockName {}

// Block element
.blockName__elementName {}

// Block Modifier
.blockName--modifierTwo {}

 
/* Good */
----------
// Block
.block-name {}

// Block Element
.block-name__element-name {}
     
// Block Modifier
.block-name--modifier-two {}

</pre>

### Namespaces
Namespcing every class allows us to ascertain exactly what kind of job a class might have, how and where we might be able to reuse it (if at all), whether or not we can modify it, and much more. (Read more in [here](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/)).

* **o-**: Signify that something is an Object, and that it may be used in any number of unrelated contexts to the one you can currently see it in. Making modifications to these types of class could potentially have knock-on effects in a lot of other unrelated places. Tread carefully.

	Format:
	<pre>
	
	.o-object-name[<element>|<modifier>] {}
	
	</pre>
	
	Example:
	
	<pre>
	
	.o-layout {}
		
	.o-layout__item {}
		
	.o-layout--fixed {}
	
	</pre>

* **Component Namespaces: c-**: Signify that something is a Component. This is a concrete, implementation-specific piece of UI. All of the changes you make to its styles should be detectable in the context you’re currently looking at. Modifying these styles should be safe and have no side effects.

	Format:
	
	<pre>
	
	.c-component-name[<element>|<modifier>] {}
	
	</pre>
	
	Example:
	
	<pre>
	
	.c-modal {}

  	.c-modal__title {}

	.c-modal--gallery {}
	
	</pre>

* **Utility Namespaces: u-**: Signify that this class is a Utility class. It has a very specific role (often providing only one declaration) and should not be bound onto or changed. It can be reused and is not tied to any specific piece of UI.

	Format:
	
	<pre>
	
	.u-utility-name {}
	
	</pre>
	
	Example:
	
	<pre>
		
	.u-clearfix {}
		
	</pre>

* **Theme Namespaces: t-**: Signify that a class is responsible for adding a Theme to a view. It lets us know that UI Components’ current cosmetic appearance may be due to the presence of a theme.

	Format:
	
	<pre>
	
	.t-theme-name {}
	
	</pre>
	
	Example:
	
	<pre>
		
	.t-light {}
		
	</pre>

* **Stateful Namespaces: is-/has**: Signify that the piece of UI in question is currently styled a certain way because of a state or condition.

	Format:
	
	<pre>
	
	.[is|has]-state {}
	
	</pre>
	
	Example:
	
	<pre>
		
	.is-open {}

	.has-dropdown {}
		
	</pre>

* **JavaScriptNamespaces:js-**: Signify that this piece of the DOM has some behaviour acting upon it, and that JavaScript binds onto it to provide that behaviour.
 
	Format:
		
	<pre>
		
	.js-component-name {}
		
	</pre>
		
	Example:
		
	<pre>
		
	.js-modal {}
			
	</pre>

* **Hack Namespaces: _**: Signify that this class is the worst of the worst—a hack! Sometimes, although incredibly rarely, we need to add a class in our markup in order to force something to work. If we do this, we need to let others know that this class is less than ideal, and hopefully temporary (i.e. do not bind onto this).

	Format:
	
	<pre>
	
	._<namespace>hack-name {}
	
	</pre>
	
	Example:
	
	<pre>
		
	._c-footer-mobile {}
			
	</pre>

* **QA Namespaces: qa-**: *TBD* (Signify that a QA or Test Engineering team is running an automated UI test which needs to find or bind onto these parts of the DOM. Like the JavaScript namespace, this basically just reserves hooks in the DOM for non-CSS purposes.)
	
	Format:
	
	<pre>
	
	.qa-node-name {}
	
	</pre>
	
	Example:
	
	<pre>
		
	.qa-error-login {}
				
	</pre>

