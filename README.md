# The jQuery Validation Plugin

This is a basic walkthrough for the [jQuery Validation Plugin](https://jqueryvalidation.org/). The assumed reader is someone who knows the basics of HTML, JavaScript, and jQuery but has probably never used a jQuery plugin before. By the end of the walkthrough you should have a good grasp of how to use the jQuery Validation Plugin to validate forms.

[Contributions](https://github.com/timothyjellison/jquery-validation-plugin-tutorial/pulls) and [critique](https://github.com/timothyjellison/jquery-validation-plugin-tutorial/issues) are very welcome.

## What does the jQuery Validation Plugin do?

Feel free to skip over this if you already know what validation means.

The jQuery Validation Plugin (JVP) validates HTML forms. Validating forms keeps users from submitting invalid data to the server. If you're using HTML forms to gather data from your users, you'll want to add some validation to ensure the data is sent to you in a format you can actually use.

A form is considered 'valid' if it contains only the types of information you (the developer) want it to. For example, if there's a full name in the full name field and a phone number in the phone number field and an email address in the email address field, JVP considers the form valid and allows the user to submit the form. But if a user provides bad information—phone numbers where the email should be, a bunch of puncuation where the name should be—JVP will disable the submit button and give the user visual and textual cues to correct his or her mistake. As the user edits their data, JVP will check repeatedly to see whether the data has become valid yet. Once the data has been edited so JVP considers it valid, the user will be allowed to submit the form.

## Inclusion &amp; Instantiation

With any jQuery plugin, you have to **include** and then **instantiate** it.

You **include** JVP by adding a script tag to your html doc. There are two easy ways to include JVP in your project. With either of these two methods, be sure to include jQuery **before** including JVP. JVP (like all other jQuery plugins) is useless without jQuery.

**Method 1)** Provide a link to a CDN in your html's `<head>` tag. Using a CDN (Content Delivery Network) saves you the trouble of storing jQuery and JVP locally ([in addition to a lot of other advantages](https://www.sitepoint.com/7-reasons-to-use-a-cdn/)).

```html
<head>
  <!-- jquery first -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.0.0-beta1/jquery.min.js"></script>
  <!-- jquery validation plugin second -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.15.0/jquery.validate.min.js"></script>
</head>
```

**Method 2)** Download the source files from [https://jqueryvalidation.org/](https://jqueryvalidation.org/) and provide script references in your html's `<head>` tag.

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

For example, say we have a form with ID `myForm`:

```html
<form id="myForm" [...]>[...]</form>
```

To instantiate JVP for this form, in a `<script>` tag we'll need to select `#myForm` with jQuery and call the `validate()` method on it, like so:

```javascript
$("#myForm").validate();
```

Your form is now being validated by JVP. This is the bare minimum for including and instantiating JVP. There are more advanced methods

## JVP's Three Methods

JVP adds three methods to jQuery: `validate()`, `valid()`, and `rules()`. That's it. If you master these three methods you'll know pretty much everything there is to know about JVP. I'll be brief on `valid()` for now, then lay out `validate()` in detail, and save a detailed explanation of `rules()` for later.

### The `valid()` method

Call this method on a jQuery selected form or form element to test whether it is valid.

```javascript
$("element").valid();
```

How does JVP know whether or not an element is valid? By looking at the `rules` specified in the `validate()` method's options object. In other words, `valid()` doesn't have any real use outside of the `validate()` method's context. So let's move on to `validate()` and save further exploration of `valid()` for later. We can do most of our work without it.

### The `validate()` method

All JVP validation begins with the `validate()` method. `validate()` accepts an object (**the options object**) as its parameter. Though it isn't the _only_ way to set up your validation procedures, in my opinion the options object is the _best_ way.

Here's a quick example of how JVP can be used to require that an input be non-empty:

```html
<form id="myForm" [...]>
  <input type="text" name="myText" [...] />
  <input type="submit" [...] />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    myText: {
      required: true,
    },
  },
});
```

Let's break this down. I select the form with jQuery and call the validate method on it. I pass an object into validate as an argument. The first property in my object tells JVP "Hey, I've got some rules ready that this form needs to follow." Inside the rules property is another object that tells JVP which element to add a rule to. And inside _that_ property is another object that specifies the rule itself.

Some things to notice in the above example. 1) Your options object is going to contain a lot of objects, and those objects will probably also contain some objects. Get used to this nested structure. 2) `rules` contains a list of the elements' _name_ attributes. Usually with jQuery we target elements using their IDs or classes, but in this specific context we're going to use names.

---

It is possible to validate using inline attributes on your form elements rather than by specifying an options object. **However**, this violates the principle of separation of concerns and is more difficult to maintain. Here's how the previous example would work with inline attributes.

```html
<form id="myForm" [...]>
  <input type="text" name="myText" [...] required />
  <input type="submit" [...] />
</form>
```

```javascript
$("#myForm").validate();
```

That said, it's better practice to use the options object than inline attributes, so we won't be covering how to use inline attributes in the rest of this tutorial.

### The `rules()` method

The `rules()` method returns, adds, and removes rules to and from form elements. We're going to wait to discuss this further. This will make a lot more sense once you've gotten to know the `validate()` method, so let's wait before looking at this any further.

## The `validate()` options object

The `validate()` method takes an object as its sole parameter. We refer to this parameter throughout this tutorial as _the options object_ because 1) it specifies all of our form's configuration settings up front and 2) that's how JVP refers to it in its own code. The options object can contain any combination of 25 different properties. Here's a comprehensive list, followed by a quick overview of all 25 properties. Each property explanation builds on the ones that came before, so you'll probably get the most out of this section if you read it sequentially.

```javascript
debug | rules | ignore | messages | submitHandler;
invalidHandler | groups | onsubmit | onfocusout | onkeyup;
onclick | focusInvalid | focusCleanup | errorClass | validClass;
errorElement | wrapper | errorLabelContainer | errorContainer | showErrors;
errorPlacement | success | highlight | unhighlight | ignoreTitle;
```

---

---

---

### `debug`

A simple boolean, if the debug property is set to true it will keep your form from submitting and redirecting your browser to the wherever your form's `action` attribute is pointed. This makes testing your validation features _much_ easier. By default `debug` is set to false.

```javascript
$("#myForm").validate({
  debug: true,
});
```

Once your form is ready to ship, make sure to set debug to false or just delete the whole property.

---

### `rules`

The rules property is one of the most important things to know about JVP. You can get a ton of value out of JVP just by knowing how to use this one property. The rules key takes as its value an object specifying validation behaviors for specific form elements. The keys in our rules value object are **the names of the form elements you wish to target**. For example, here we'll target two input elements (firstName and lastName) to make sure they aren't empty.

```html
<form action="#" method="GET" id="myForm">
  <label for="firstNameId">First Name</label>
  <input id="firstNameId" name="firstName" type="text" />
  <label for="lastNameId">Last Name</label>
  <input id="lastNameId" name="lastName" type="text" />
  <input type="submit" />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    firstName: "required",
    lastName: "required",
  },
});
```

With these validation rules in place, our form won't be able to submit unless the user has entered values into both fields.

Notice that the keys in our rules value object point directly to the inputs' name attributes, **not** their id attributes. To illustrate the point I've given the ID and name attributes different values, though in real use they're usually the same.

Many inputs will need more than one rule applied to them. What if we want JVP to test whether an input is not empty AND is a valid email address? To accomplish this, your name key should have an **object** as its value (rather than a string, as in the example above). We'll add a required email field to the above example, and put the old firstName and lastName rules into the new object style as well.

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
    email: {
      required: true,
      email: true,
    },
  },
});
```

Now this not only enforces that the email field must be non-empty, but it also makes sure that the user enters a valid email address (must contain an "@" and some sort of ".com" type ending). This style of writing rules provides much greater flexibility and is more readable.

---

### `ignore`

Lets you specify rules to ignore. This can be useful if certain conditions had changed and a rule no longer needed to be validated. Imagine somebody clicked a checkbox specifying that they have no last name. The checkbox could trigger a toggleClass method that adds the class "ignoreMe" to the lastName input element, and then an ignore property in your options would make sure the validator ignores the lastName input.

```html
<form action="#" method="GET" id="myForm">
  <input type="checkbox" id="noLastName" name="noLastName" /><label
    for="noLastName"
    >No Last Name</label
  >
  <label for="lastName">Last Name</label>
  <input id="lastName" name="lastName" type="text" />
  <input type="submit" />
</form>
```

```javascript
$("# noLastName").bind("change", function () {
  $("# lastName").toggleClass("ignoreMe");
});

$("#myForm").validate({
  rules: {
    lastName: {
      required: true,
    },
  },
  ignore: ".ignoreMe",
});
```

Even though a required rule is specified for lastName, the validator will ignore lastName since it belongs to the ignoreMe class.

---

### `submitHandler` and `invalidHandler`

The submitHandler property runs an anonymous callback function when a valid form is successfully submitted. Basically, this lets you take control of the standard submit button functionality. Say you're in debug mode and you want to send a message to the console when a valid form is submitted.

```javascript
$("#myForm").validate({
  debug: true,
  rules: {
    firstName: {
      required: true,
    },
  },
  submitHandler: function (form) {
    console.info("form valid; submission sent");
  },
});
```

You'll notice that the callback function accepts a single parameter that represents the form. You're effectively hijacking the submit button's default functionality, so in a live context the form argument provides an easy way to explicitly tell the JVP to submit the form.

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
  },
  submitHandler: function (form) {
    console.info("form valid; submission sent");
    form.submit();
  },
});
```

You could use this to trigger an animation, a style change, a modal, or some set of calculations. Whatever you want.

`invalidHandler` is basically the same as submitHandler, except the anonymous callback function runs every time the user attempts to submit an invalid form.

```javascript
$("#myForm").validate({
  debug: true,
  rules: {
    firstName: {
      required: true,
    },
  },
  invalidHandler: function (form) {
    console.error("form invalid; submission rejected");
  },
});
```

You could use this to trigger an animation, a style change, a modal, or some set of calculations. Whatever you want.

---

### `messages`

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

### `errorClass` and `validClass`

Whenever JVP validates your form, it adds a class to each validated form element to indicate whether or not it was valid. By default, valid inputs are given class "valid" and invalid inputs are given class "error".

Say we had a simple html form with two inputs, plus a simple validation script:

```html
<form action="#" method="GET" id="myForm">
  <label for="firstName">First Name</label>
  <input id="firstName" name="firstName" type="text" />
  <label for="lastName">Last Name</label>
  <input id="lastName" name="lastName" type="text" />
  <input type="submit" />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
});
```

When JVP runs (when you hit the submit button, for example) it will add attributes to your form elements and even add some new elements. Here's our form after JVP runs:

```html
<form action="#" method="GET" id="myForm" novalidate="novalidate">
  <label for="firstName">First Name</label>
  <input
    id="firstName"
    name="firstName"
    type="text"
    aria-required="true"
    class="error"
    aria-invalid="true"
  /><label id="firstName-error" class="error" for="firstName"
    >This field is required.</label
  >
  <label for="lastName">Last Name</label>
  <input
    id="lastName"
    name="lastName"
    type="text"
    aria-required="true"
    class="error"
  /><label id="lastName-error" class="error" for="lastName"
    >This field is required.</label
  >
  <input type="submit" />
</form>
```

First, notice that JVP has added label elements after our input elements. As you work on Styling Error Messages, you'll want to take find ways to style all these new elements that will be popping in and out of the DOM.

Second, notice that JVP has added attributes to almost all of our form elements. JVP added `novalidate="novalidate"` to our form element, which turns off HTML5 validation (JPV is doing you a favor: you don't want multiple validation systems getting their wires crossed). It also added ARIA attributes to make the form a11y friendly. Read up on [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) and [form accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/forms).

But the really useful bit for our current topic is in the `class="error"` that's added to the inputs and labels. These provide a nice, semantic handles for CSS and jQuery to select. Now you can create style rules like `.error { ... };` or event listeners like `$('.error').bind('hover', function(){ ... });`

But what if you want to use a class than "error"? What if that class is already being used in your site, or you really just prefer "err" or "red_sub_small" or "foobar"? That's where the errorClass property comes in. Let's modify that script from before:

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  errorClass: "floop",
});
```

Now when you run JVP it will add `class="floop"` to your form elements rather than "error".

`validClass` is basically the same as `errorClass`. When JVP runs and finds that one of your form elements is valid, it will add `class="valid"` to that element. To take control and use your own custom class value, specify a validClass property.

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  errorClass: "floop",
  validClass: "hurray",
});
```

### `groups`

What if you want to validate two form elements but only display one error message if either is invalid? For example, say you were testing for first and last name but only wanted to write 'Please enter your full name' if either input was missing a value (rather than separate 'Please enter your first name' and 'Please enter your last name' messages). How would you accomplish that?

You would use the `groups` property.

```html
<form id="myForm" action="#" method="GET">
  <label for="fname">First Name: </label
  ><input type="text" name="fname" id="fname" /><br />
  <label for="lname">Last Name:</label
  ><input type="text" name="lname" id="lname" /><br />
  <input type="submit" />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    fname: {
      required: true,
    },
    lname: {
      required: true,
    },
  },
  groups: {
    username: "fname lname",
  },
});
```

Here the groups property is binding the `fname` and `lname` inputs into a new group called `username`. If either element doesn't meet the requirements of the `rules`, a single error message will display. Note that you'll probably want to take some control of where the error messages are placed; by default the error message attaches to the first element in the group. See `errorPlacement`

---

### `onsubmit`

This is a simple boolean (true by default) that turns validation off at the point of submission. If you want validation to take place as the user moves through the form (triggering keyup, blur, focus, click, and other events) but ultimately be able to submit whatever they want when they hit the submit button, set `onsubmit` to false.

```javascript
$("#myForm").validate({
  onsubmit: false,
});
```

Note that if `onsubmit` is set to false and you haven't set validation to be triggered by any other events (keyup, blur, etc.) then validation simply won't happen. Also, any custom `submitHandler` and `invalidHandler` functions you've specified won't run.

---

### `onfocusout`

The `onfocusout`property gives you some control over what happens when focus moves off of a form element. If you don't want any validation to occur on focus out, set it to false.

```javascript
$("#myForm").validate({
  onfocusout: false,
});
```

You can also specify a callback function to give you more control. The callback takes two arguments, element and event, which contain info about the element currently being validated and the event itself. Here we'll tell the validator to check for validity on focus out:

```javascript
$("#myForm").validate({
  rules: {
    fname: {
      required: true,
    },
    lname: {
      required: true,
    },
  },
  onfocusout: function (element, event) {
    $(element).valid();
  },
});
```

Now, instead of waiting until the submit button is hit, JVP will check the element's rules every time the user moves from one element to the next. Here we see our first practical use of the `valid()` method. Here it runs JVP validation on a single element. It also returns a boolean indicating whether the selected element is valid, so you can use it in conditional tests to control program flow. The `valid()` method is simple and powerful.

---

### `onkeyup`

Exactly the same as onfocusout, except triggered by the user pressing a key while focus is on the selected element. (Note: this includes hitting `tab`ing to move focus out of the selected element.)

```javascript
$("#myForm").validate({
  rules: {
    fname: {
      required: true,
    },
    lname: {
      required: true,
    },
  },
  onkeyup: function (element, event) {
    $(element).valid();
  },
});
```

We'll find that `onkeyup`'s `event` argument returns an object every time the event is triggered. It contains **a lot** of useful information. Here's a sample `event` object that returned after pushing the 'a' key while my `fname` input had focus:

```javascript
o.Event {
	altKey: false,
	bubbles: true,
	cancelable: true,
	char: undefined,
	charCode: 0,
	ctrlKey: false,
	currentTarget: input# fname.valid,
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
	target: input# fname.valid,
	timeStamp: 24837.850000000002,
	type: "keyup",
	view: Window,
	which: 65,
	__proto__: Object
}
```

Literally every time the event is triggered this wealth of information is returned. What can you do with it? A really useful property in this `event` object is `keyCode`. Want to only validate when numbers or letters are pressed? (Really, why should we validate just because a user pushed the shift key?) Add a condition that checks whether event.keyCode is greater than 46 and less than 91. Want to only validate when the tab key is pressed? Add a condition that checks whether event.keyCode equals 9. Play around with the properties and try to find practical use cases.

---

### `onclick`

Again, `onclick` functions just like `onfocusout` and `onkeyup` except that 1) it only applies to checkboxes and radio buttons and 2) its `event` argument returns an object with different information from what you'd find in those other events (specifically, information about the mouse, such as which button was clicked, its x and y coordinates at the time the click happened, when the click happened, etc.).

Let's make a new form with some checkboxes and radio buttons and then add a validation script that requires one of the radio buttons to be selected and uses the `onclick` property to hide the phone and fax options if the checkbox is selected.

```html
<form id="myForm" action="#" method="GET">
  <input type="radio" name="radio" value="email" id="email" /><label for="email"
    >Email</label
  >
  <input
    type="radio"
    name="radio"
    value="phone"
    id="phone"
    class="weekend"
  /><label for="phone" class="weekend">Phone</label>
  <input type="radio" name="radio" value="fax" id="fax" class="weekend" /><label
    for="fax"
    class="weekend"
    >Fax</label
  >
  <input type="checkbox" id="checkbox" name="checkbox" checked /><label
    for="checkbox"
    >OK to contact on weekends?</label
  >
  <input type="submit" />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    radio: {
      required: true,
    },
  },
  onclick: function (element, event) {
    if (element.id == "checkbox") {
      if (element.checked) {
        $(".weekend").show();
      } else {
        $(".weekend").hide();
      }
    }
  },
});
```

---

### `focusInvalid`

After a submit has been attempted and JVP has determined that the form is invalid, focus is given by default to the first invalid element. You can turn this behavior off by setting focusInvalid to false. The focus will stay wherever it was at submit (usually this is on the submit button itself).

```javascript
$("#myForm").validate({
  focusInvalid: false,
});
```

---

### `focusCleanup`

Remember that whenever JVP validates your form it adds lots of extra classes and labels to your form elements. All these extra goodies might be distracting to a user as they go through and correct the form. `focusCleanup` removes the error classes and error messages as the invalid elements are given focus. If the user corrects the element and makes it valid, the error classes and messages will remain removed. If the user does not correct the element, the error classes and messages will return when the element loses focus.

```javascript
$("#myForm").validate({
  focusCleanup: true,
});
```

Remember that this means lots of DOM elements will be shifting around. Get your styling right so this isn't visually distracting.

---

### `errorElement` and `wrapper`

By default, error messages are added to the DOM inside of a `<label>` element. You can override this by specifying a value for `errorElement`. You can put them in `<em>` tags, for example.

```javascript
$("#myForm").validate({
  errorElement: "em",
});
```

Where before you would have gotten an error message like this:

```html
<label id="firstName-error" class="error" for="firstName"
  >This field is required.</label
>
```

Now you get this:

```html
<em id="firstName-error" class="error">This field is required.</em>
```

`wrapper`, on the other hand, wraps the error element in another tag. Combine this with `errorElement` if you wish:

```javascript
$("#myForm").validate({
  errorElement: "em",
  wrapper: "strong",
});
```

With the default settings you would have gotten an error message like this:

```html
<label id="firstName-error" class="error" for="firstName"
  >This field is required.</label
>
```

But now you get this:

```html
<strong>
  <em id="firstName-error" class="error">This field is required.</em>
</strong>
```

---

### `errorLabelContainer` and `errorContainer`

By default, JVP adds any error element to the DOM as **the next sibling of the invalid form element**. Using `errorLabelContainer`, you can specify **a different element to add the error labels to as children**. Let's say we wanted all our error messages to go into a message box. We would first create a div somewhere in our html and give it an ID for jQuery to grab onto, then set errorLabelContainer to that ID.

```html
<label for="firstName">First Name</label>
<input id="firstName" name="firstName" type="text" />
<label for="middleName">Middle Name</label>
<input id="middleName" name="middleName" type="text" />
<label for="lastName">Last Name</label>
<input id="lastName" name="lastName" type="text" />
<input type="submit" />

<div id="myLabelContainer"></div>
```

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    middleName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  errorLabelContainer: "# myLabelContainer",
});
```

After the form validates the labels are all appended to # myLabelContainer, like so:

```html
<form action="#" method="GET" id="myForm" novalidate="novalidate">
  <label for="firstName">First Name</label>
  <input
    id="firstName"
    name="firstName"
    type="text"
    aria-required="true"
    class="error"
    aria-invalid="true"
  />
  <label for="middleName">Middle Name</label>
  <input
    id="middleName"
    name="middleName"
    type="text"
    aria-required="true"
    class="error"
  />
  <label for="lastName">Last Name</label>
  <input
    id="lastName"
    name="lastName"
    type="text"
    aria-required="true"
    class="error"
  />
  <input type="submit" />

  <div id="myLabelContainer">
    <label id="firstName-error" class="error" for="firstName"
      >This field is required.</label
    >
    <label id="middleName-error" class="error" for="middleName"
      >This field is required.</label
    >
    <label id="lastName-error" class="error" for="lastName"
      >This field is required.</label
    >
  </div>
</form>
```

_[[I could use some help unpacking ```errorContainer``` does. It's unclear to me how it differs from errorLabelContainer.]]_

---

### `showErrors`

The showErrors property gives you convenient access to all of the validation information spit out when a form is deemed invalid. Want a quick way to know the number and type of errors the user needs to address? It's readily available with `showErrors`.

`showerrors` takes as its value a callback function that accepts two arguments, **errorMap** and **errorList**, each of which gives you different information about your form's validation settings.

errorMap is an object, basically just a set of key-value pairs where the key is the error-causing input's name and the value is the error message it displays.

errorList is a little more complex. It's an array of objects each containing three properties: 1) .element, which is just a pointer to the error-causing input, 2) .message, the error message string, and 3) .method, a string that names the particular rule that triggered the validation warning. Here's an example:

```html
<form action="#" method="GET" id="myForm">
  <label for="firstName">First Name</label>
  <input id="firstName" name="firstName" type="text" />
  <label for="lastName">Last Name</label>
  <input id="lastName" name="lastName" type="text" />
  <input type="submit" />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  showErrors: function (errorMap, errorList) {
    console.log("errorMap: ", errorMap);
    errorList.forEach(function (error, index) {
      console.log(
        "errorList index " +
          index +
          ": " +
          "element -- " +
          error.element +
          ", " +
          "message -- " +
          error.message +
          ", " +
          "method -- " +
          error.method
      );
    });
  },
});
```

If you run this and try to submit the form without filling in any values for firstName or lastName, the console will output the following:

```
errorMap: Object {firstName: "This field is required.", lastName: "This field is required."}
errorList index 0: element -- [object HTMLInputElement], message -- This field is required., method -- required
errorList index 1: element -- [object HTMLInputElement], message -- This field is required., method -- required
```

_[[I'm struggling to find a good illustrative use case for this. Would appreciate [contributions](https://github.com/timothyjellison/jquery-validation-plugin-tutorial/pulls).]]_

---

### `errorPlacement`

By default, error labels are appended to the element that cause the error. `errorPlacement` lets you hijack that functionality, allowing you to place error labels wherever you choose.

Like `showerrors`, `errorPlacement` takes as its value a callback function, which requires two arguments: error (the created error label as a jQuery object) and element (the invalid element as a jQuery object).

Lets say we want to put our errors into a div with ID 'errorList'. Here's how we'd do it:

```html
<form action="#" method="GET" id="myForm">
  <label for="firstName">First Name</label>
  <input id="firstName" name="firstName" type="text" />
  <label for="lastName">Last Name</label>
  <input id="lastName" name="lastName" type="text" />
  <input type="submit" />
</form>

<div id="errorList"></div>
```

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  errorPlacement: function (error, element) {
    error.appendTo("# errorList");
  },
});
```

---

### `success`

So far we've only added labels to errors. But what if you want to tell the user that they're getting things right? Use the `success` property to send the user messages when a form element is valid.

There are two ways to use `success`, one where you provide a string and another where you provide a callback function. The string just adds a class of your choice to the valid element.

```html
<form action="#" method="GET" id="myForm">
  <label for="firstName">First Name</label>
  <input id="firstName" name="firstName" type="text" />
  <label for="lastName">Last Name</label>
  <input id="lastName" name="lastName" type="text" />
  <input type="submit" />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  success: "mySuccessClass",
});
```

Now 'mySuccessClass' will be toggled on and off your elements as they are found valid or invalid, respectively.

The callback function approach provides some more control. The function takes two arguments: `label`, a pointer to the error label element, and `element`, a pointer to the invalid element.

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  success: function (label, element) {
    label.text("Ok!");
    alert(element.val());
  },
});
```

---

### `highlight` and `unhighlight`

By default, jQuery adds the errorClass to any form elements found invalid, a process called _highlighting_. (The default errorClass is simply "error", but as you saw above you can change this with the errorClass property.)

The `highlight` property gives you more control over the highlighting process. It's value is a callback function that takes three arguments: element, errorClass, and validClass. element, as you've seen in other properties above, is a pointer to the invalid element. `errorClass` and `validClass` give you the name of the currently set errorClass and validClass as strings.

Let's look at an example:

```html
<form action="#" method="GET" id="myForm">
  <label for="firstName">First Name</label>
  <input id="firstName" name="firstName" type="text" />
  <label for="lastName">Last Name</label>
  <input id="lastName" name="lastName" type="text" />
  <input type="submit" />
</form>
```

```javascript
$("#myForm").validate({
  rules: {
    firstName: {
      required: true,
    },
    lastName: {
      required: true,
    },
  },
  highlight: function (element, errorClass, validClass) {
    $(element).addClass(errorClass);
    alert(errorClass);
  },
});
```

---

### `ignoreTitle`

If your form elements have `title` attributes (usually used in forms to create helpful tooltips when the user hovers over the element), JVP will pull those attribute values into the error labels. This is obviously a problem, so be sure to set `ignoreTitle` to true.

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
  	ignoreTitle: true,
  }
});
```
