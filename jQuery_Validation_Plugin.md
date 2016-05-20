#The jQuery Validation Plugin

##Inclusion &amp; instantiation

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

For example, say I have a form with ID ```myForm```:

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





##The ```validate()``` method

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

That said, it's better practice to use the config object than inline attributes, so I won't be covering how to use inline attributes in the rest of this tutorial.






##validate() config object properties

Here's a comprehensive list of the config object's 25 properties:

```javascript
debug           |  rules         |  ignore               |  messages        |  submitHandler
invalidHandler  |  groups        |  onsubmit             |  onfocusout      |  onkeyup
onclick         |  focusInvalid  |  focusCleanup         |  errorClass      |  validClass
errorElement    |  wrapper       |  errorLabelContainer  |  errorContainer  |  showErrors
errorPlacement  |  success       |  highlight            |  unhighlight     |  ignoreTitle
```

What follows is a quick overview of all 25 properties. Each builds on what came before it, so while you can use this section as a reference, you'll probably get the most out of them if you read them as a sequence.

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

When JVP runs (when you hit the submit button, for example) it will add several attributes

---


###validClass

---


###groups

---


###onsubmit

---


###onfocusout

---


###onkeyup

---


###onclick

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