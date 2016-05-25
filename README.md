#The jQuery Validation Plugin

A basic walkthrough for how to use the [jQuery Validation Plugin](https://jqueryvalidation.org/). The assumed audience is someone who knows the basics of HTML forms, JavaScript, and jQuery but has probably never used a jQuery plugin before.

[Contributions](https://github.com/timothyjellison/jquery-validation-plugin-tutorial/pulls) and [critique](https://github.com/timothyjellison/jquery-validation-plugin-tutorial/issues) are very welcome.

##Inclusion &amp; Instantiation

There are two easy ways to include the jQuery Validation Plugin (JVP) in your project. In both cases, be sure to include jQuery **before** including JVP.

1. Provide a link to a CDN in your html's ```<head>``` tag. This way you don't have to download anything or bother asking the client to store the scripts locally.

  ```html
  <head>
  	<!-- jquery first -->
  	<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.0.0-beta1/jquery.min.js"></script>
  	<!-- jquery validation plugin second -->
  	<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.15.0/jquery.validate.min.js"></script>
  </head>
  ```

2. Download the source files from [https://jqueryvalidation.org/](https://jqueryvalidation.org/) and provide script references in your html's ```<head>``` tag.

  ```html
  <head>
  	<!-- jquery first -->
  	<script src="jquery.min.js"></script>
  	<!-- jquery validation plugin second -->
  	<script src="jquery.validate.min.js"></script>
  </head>
  ```


---

You instatiate an instance of JVP by calling the validate() method on a jQuery selection of your form. Easier done than said.

For example, say we have a form with ID ```myForm```:

```html
<form id="myForm" [...] >
	[...]
</form>
```

To instantiate JVP for this form, in a ```<script>``` tag I'll need to select ```#myForm``` with jQuery and call the ```validate()``` method on it, like so:

```javascript
$('#myForm').validate();
```

This is the bare minimum for installing and instantiating JVP.




##The Three Methods

The JVP only adds three methods to jQuery: ```validate()```, ```valid()```, and ```rules()```. That's it. Master these three and you'll know pretty much everything there is to know about the JVP. I'll talk briefly about ```valid()``` now, then lay out ```validate()``` in detail, and save an explanation of ```rules()``` for much later.

###The ```valid()``` method

Call this method on a selected form or form element to test whether it is valid.

```javascript
$(element).valid();
```

How does JVP test whether or not an element is valid? By looking at the ```rules``` specified in the ```validate()``` method's config object. ```valid()``` doesn't have any real use outside of the ```validate()``` method's context, so let's move on to ```validate()``` and save any further exploration of ```valid()``` for later. We can do most of our work without it.


###The ```validate()``` method

All JVP validation begins with the ```validate()``` method. ```validate()``` accepts an object (the config object) as its parameter. Though it isn't the *only* way to set up your validation procedures, the config object is the **best** way.

Here's a quick example of how JVP can be used to require that an input be non-empty:

```html
<form id="myForm" [...] >
	<input type="text" name="myText" [...] />
	<input type="submit" [...] />
</form>
```

```javascript
$('#myForm').validate({
	// this first property in my object
	// sets the rules for my validation procedures
	rules: {
		// an object nested in my first object
		// targets the specific element to add a rule to
		myText: {
			// another object nested inside the object
			// (noticing a pattern?)
			// specifies the actual rule
			required: true
		}
	}
});
```

It is possible to validate using inline attributes on your form elements rather than by specifying a config object. **However**, this violates the principle of separation of concerns and is more difficult to maintain. Here's how the previous example would work with inline attributes.

```html
<form id="myForm" [...] >
	<input type="text" name="myText" [...] required />
	<input type="submit" [...] />
</form>
```

```javascript
$('#myForm').validate();
```

That said, it's better practice to use the config object than inline attributes, so we won't be covering how to use inline attributes in the rest of this tutorial.

###The ```rules()``` method

The ```rules()``` method returns, adds, and removes rules to/from form elements. We're going to wait to discuss this further. It will make a lot more sense once you've gotten to know the ```validate()``` method.




##```validate()``` config object properties

The ```validate()``` method takes an object as its sole parameter. We'll refer to this parameter throughout this tutorial as the config object because it specifies all of our form's configuration settings up front. The config object can contain 25 different properties. Here's a comprehensive list, followed by a quick overview of all 25 properties. Each property explanation builds on the ones that came before, so you'll probably get the most out of this section if you read it sequentially.


```javascript
debug           |  rules         |  ignore               |  messages        |  submitHandler
invalidHandler  |  groups        |  onsubmit             |  onfocusout      |  onkeyup
onclick         |  focusInvalid  |  focusCleanup         |  errorClass      |  validClass
errorElement    |  wrapper       |  errorLabelContainer  |  errorContainer  |  showErrors
errorPlacement  |  success       |  highlight            |  unhighlight     |  ignoreTitle
```


---

---

---

###debug

A simple boolean, the debug property will keep your form from submitting in order to make testing your validation features easier. It will save you a lot of hitting the back and refresh buttons.

```javascript
$('#myForm').validate({
  debug: true
});
```

Make sure to set debug to false (or just delete the whole property) once your form is ready to ship.

---

###rules

The rules property is one of the most important things to know about JVP. You can get a ton of value out of JVP just by knowing how to use this one property. The rules key takes as its value an object specifying validation behaviors for specific form elements. The keys in our rules value object are **the names of the form elements you wish to target**. For example, here we'll target two input elements (firstName and lastName) to make sure they aren't empty.

```html
<form action="#" method="GET" id="myForm">
	<label for="firstNameId">First Name</label>
	<input id="firstNameId" name="firstName" type="text">
	<label for="lastNameId">Last Name</label>
	<input id="lastNameId" name="lastName" type="text">
	<input type="submit">
</form>
```

```javascript
$('#myForm').validate({
  rules: {
	firstName: "required",
	lastName: "required"
  }
});
```

With these validation rules in place, our form won't be able to submit unless the user has entered values into both fields.

Notice that the keys in our rules value object point directly to the inputs' name attributes, **not** their id attributes. To illustrate the point I've given the ID and name attributes different values, though in real use they're usually the same.

Many inputs will need more than one rule applied to them. What if we want JVP to test whether an input is not empty AND is a valid email address? To accomplish this, your name key should have an **object** as its value (rather than a string, as in the example above). We'll add a required email field to the above example, and put the old firstName and lastName rules into the new object style as well.

```javascript
$('#myForm').validate({
  rules: {
	firstName: {
		required: true
	},
	lastName: {
		required: true
	},
	email: {
	  required: true,
	  email: true
	}
  }
});
```

Now this not only enforces that the email field must be non-empty, but it also makes sure that the user enters a valid email address (must contain an "@" and some sort of ".com" type ending). This style of writing rules provides much greater flexibility and is more readable.

---


###ignore

Lets you specify rules to ignore. This can be useful if certain conditions had changed and a rule no longer needed to be validated. Imagine somebody clicked a checkbox specifying that they have no last name. The checkbox could trigger a toggleClass method that adds the class "ignoreMe" to the lastName input element, and then an ignore property in your config object would make sure the validator ignores the lastName input.

```html
<form action="#" method="GET" id="myForm">
	<input type="checkbox" id="noLastName" name="noLastName"><label for="noLastName">No Last Name</label>
	<label for="lastName">Last Name</label>
	<input id="lastName" name="lastName" type="text">
	<input type="submit">
</form>
```

```javascript
$('#noLastName').bind("change", function(){
	$('#lastName').toggleClass('ignoreMe');
});

$('#myForm').validate({
  rules: {
	lastName: {
		required: true
	}
  },
  ignore: ".ignoreMe"
});
```

Even though a required rule is specified for lastName, the validator will ignore lastName since it belongs to the ignoreMe class.


---


###submitHandler

The submitHandler property runs an anonymous callback function when a valid form is successfully submitted. Basically, this lets you take control of the standard submit button functionality. Say you're in debug mode and you want to send a message to the console when a valid form is submitted.

```javascript
$('#myForm').validate({
  debug: true,
  rules: {
	firstName: {
		required: true
	}
  },
  submitHandler: function(form) {
	console.info('form valid; submission sent');
  }
});
```

You'll notice that the callback function accepts a single parameter that represents the form. You're effectively hijacking the submit button's default functionality, so in a live context the form argument provides an easy way to explicitly tell the JVP to submit the form.

```javascript
$('#myForm').validate({
  rules: {
	firstName: {
		required: true
	}
  },
  submitHandler: function(form) {
	console.info('form valid; submission sent');
	form.submit();
  }
});
```

You could use this to trigger an animation, a style change, a modal, or some set of calculations. Whatever you want.

---


###invalidHandler

Basically the same as submitHandler, except the anonymous callback function runs every time the user attempts to submit an invalid form.

```javascript
$('#myForm').validate({
  debug: true,
  rules: {
	firstName: {
		required: true
	}
  },
  invalidHandler: function(form) {
	console.error('form invalid; submission rejected');
  }
});
```

You could use this to trigger an animation, a style change, a modal, or some set of calculations. Whatever you want.

---


###messages

JVP comes packaged with some default validation responses to prompt the user for valid information ("This field is required.", "Please enter a valid email address.", etc.), but you'll probably want to come up with your own messages. As with the rules property, the messages property takes an object, and that's object's keys match up to input element names.

```javascript
$('#myForm').validate({
  rules: {
	firstName: {
		required: true
	},
	lastName: {
		required: true
	},
	email {
		required: true,
	 	email: true
	}
  },
  messages: {
  	firstName: "Please enter first name.",
  	lastName: "Please enter last name.",
  	email {
  		required: "Please enter email address.",
  		email: "Invalid email address!"
  	}
  }
});
```

Notice in the email messages that you can tailor your messages to the individual behaviors you've specified in your rules.

---


###errorClass

Whenever JVP validates your form, it adds a class to each validated form element to indicate whether or not it was valid. By default, valid inputs are given class "valid" and invalid inputs are given class "error".

Say we had a simple html form with two inputs, plus a simple validation script:

```html
<form action="#" method="GET" id="myForm">
	<label for="firstName">First Name</label>
	<input id="firstName" name="firstName" type="text">
	<label for="lastName">Last Name</label>
	<input id="lastName" name="lastName" type="text">
	<input type="submit">
</form>
```

```javascript
$('#myForm').validate({
  rules: {
	firstName: {
		required: true
	},
	lastName: {
		required: true
	}
  }
});
```

When JVP runs (when you hit the submit button, for example) it will add attributes to your form elements and even add some new elements. Here's our form after JVP runs:

```html
<form action="#" method="GET" id="myForm" novalidate="novalidate">
	<label for="firstName">First Name</label>
	<input id="firstName" name="firstName" type="text" aria-required="true" class="error" aria-invalid="true"><label id="firstName-error" class="error" for="firstName">This field is required.</label>
	<label for="lastName">Last Name</label>
	<input id="lastName" name="lastName" type="text" aria-required="true" class="error"><label id="lastName-error" class="error" for="lastName">This field is required.</label>
	<input type="submit">
</form>
```

First, notice that JVP has added label elements after our input elements. As you work on Styling Error Messages, you'll want to take find ways to style all these new elements that will be popping in and out of the DOM.

Second, notice that JVP has added attributes to almost all of our form elements. JVP added ```novalidate="novalidate"``` to our form element, which turns off HTML5 validation (JPV is doing you a favor: you don't want multiple validation systems getting their wires crossed). It also added ARIA attributes to make the form a11y friendly. Read up on [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) and [form accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/forms).

But the really useful bit for our current topic is in the ```class="error"``` that's added to the inputs and labels. These provide a nice, semantic handles for CSS and jQuery to select. Now you can create style rules like ```.error { ... };``` or event listeners like ```$('.error').bind('hover', function(){ ... });```

But what if you want to use a class than "error"? What if that class is already being used in your site, or you really just prefer "err" or "red_sub_small" or "foobar"? That's where the errorClass property comes in. Let's modify that script from before:

```javascript
$('#myForm').validate({
  rules: {
	firstName: {
		required: true
	},
	lastName: {
		required: true
	}
  },
  errorClass: "floop"
});
```

Now when you run JVP it will add ```class="floop"``` to your form elements rather than "error".

---


###validClass

Basically the same as above. When JVP runs and finds that one of your form elements is valid, it will add ```class="valid"``` to that element. To take control and use your own custom class value, specify a validClass property.

```javascript
$('#myForm').validate({
  rules: {
	firstName: {
		required: true
	},
	lastName: {
		required: true
	}
  },
  errorClass: "floop",
  validClass: "hurray"
});
```

---


###groups

What if you want to validate two form elements but only display one error message if either is invalid? For example, say you were testing for first and last name but only wanted to write 'Please enter your full name' if either input was missing a value (rather than separate 'Please enter your first name' and 'Please enter your last name' messages). How would you accomplish that?

You would use the ```validate()``` config object's ```groups``` property.

```html
<form id="myForm" action="#" method="GET">
	<label for="fname">First Name: </label><input type="text" name="fname" id="fname"/><br>
	<label for="lname">Last Name:</label><input type="text" name="lname" id="lname"/><br>
    <input type="submit"/>
</form>
```


```javascript
$("#myForm").validate({
    rules: {
        fname: {
        	required: true
        },
        lname: {
			required: true
        }
    },
	groups: {
		username: "fname lname"
	}
});

```

Here the groups property is binding the ```fname``` and ```lname``` inputs into a new group called ```username```. If either element doesn't meet the requirements of the ```rules```, a single error message will display. Note that you'll probably want to take some control of where the error messages are placed; by default the error message attaches to the first element in the group. See ```errorPlacement```




---


###onsubmit

This is a simple boolean (true by default) that turns validation off at the point of submission. If you want validation to take place as the user moves through the form (triggering keyup, blur, focus, click, and other events) but ultimately be able to submit whatever they want when they hit the submit button, set ```onsubmit``` to false.

```javascript
$("#myForm").validate({
  onsubmit: false
});
```

Note that if ```onsubmit``` is set to false and you haven't set validation to be triggered by any other events (keyup, blur, etc.) then validation simply won't happen. Also, any custom ```submitHandler``` and ```invalidHandler``` functions you've specified won't run.



---


###onfocusout

The ```onfocusout```property gives you some control over what happens when focus moves off of a form element. If you don't want any validation to occur on focus out, set it to false.

```javascript
$("#myForm").validate({
    onfocusout: false
});
```

You can also specify a callback function to give you more control. The callback takes two arguments, element and event, which contain info about the element currently being validated and the event itself. Here we'll tell the validator to check for validity on focus out:

```javascript
$("#myForm").validate({
    rules: {
        fname: {
        	required: true
        },
        lname: {
			required: true
        }
    },
    onfocusout: function(element, event){
    	$(element).valid();
    }
});
```

Now, instead of waiting until the submit button is hit, JVP will check the element's rules every time the user moves from one element to the next. Here we see our first practical use of the ```valid()``` method. Here it runs JVP validation on a single element. It also returns a boolean indicating whether the selected element is valid, so you can use it in conditional tests to control program flow. The ```valid()``` method is simple and powerful.

---


###onkeyup

Exactly the same as onfocusout, except triggered by the user pressing a key while focus is on the selected element. (Note: this includes hitting ```tab```ing to move focus out of the selected element.)

```javascript
$("#myForm").validate({
    rules: {
        fname: {
        	required: true
        },
        lname: {
			required: true
        }
    },
    onkeyup: function(element, event){
    	$(element).valid();
    }
});
```

We'll find that ```onkeyup```'s ```event``` argument returns an object every time the event is triggered. It contains **a lot** of useful information. Here's a sample ```event``` object that returned after pushing the 'a' key while my ```fname``` input had focus:

o.Event {
	altKey: false,
	bubbles: true,
	cancelable: true,
	char: undefined,
	charCode: 0,
	ctrlKey: false,
	currentTarget: input#fname.valid,
	data: undefined,
	delegateTarget: form#myForm,
	detail: 0,
	eventPhase: 3,
	handleObj: Object,
	isDefaultPrevented: pa(),
	jQuery30010751047619751209: true,
	key: undefined,
	keyCode: 65,
	metaKey: false,
	originalEvent: KeyboardEvent,
	relatedTarget: undefined,
	shiftKey: false,
	target: input#fname.valid,
	timeStamp: 24837.850000000002,
	type: "keyup",
	view: Window,
	which: 65,
	__proto__: Object
}

A really useful property in this ```event``` object is ```keyCode```. Want to only validate when numbers or letters are pressed? (Why should we validate just because a user pushed the shift key?) Add a condition that checks whether event.keyCode is greater than 46 and less than 91. Want to only validate when the tab key is pressed? Add a condition that checks whether event.keyCode equals 9. Play around with these and try to find practical use cases.

---


###onclick

Again, ```onclick``` functions just like ```onfocusout``` and ```onkeyup``` except that 1) it only applies to checkboxes and radio buttons and 2) its ```event``` argument returns an object with very different information from what you'd find in those other events.



---


###focusInvalid

---


###focusCleanup

---



###errorElement

---


###wrapper

---


###errorLabelContainer

---


###errorContainer

---


###showErrors

---


###errorPlacement

---


###success

---


###highlight

---


###unhighlight

---


###ignoreTitle



##Styling Error Messages