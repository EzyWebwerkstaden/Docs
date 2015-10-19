### Javascript

## Viewmodel
We are using knockoutjs as our binding library between html and viewmodels in javascript.
So that's our standard here.

The standard viewmodel pattern should look like this.

```javascript
var ezy = ezy || {};
ezy.page = ezy.page || {};

ezy.page.mycomponent = function(){
	 priv = {},
	 pub ={};
	 	 	 	 
	 pub.init = function(config){
		 priv.config = config;
		 var viewModel = priv.viewModel();
	 }
	 
	 priv.viewModel = function(){
		 var inner = {};
		 inner.thatproperty = ko.observable();
		 return inner;
	 }
	 
	 priv.myprivatemethod = function(){
		 
	 }
	 
	 priv.config = {};
	 
	 
	 return pub;
	}
```

And then we can bind it with knockout.

```javascript

var viewModel = ezy.page.mycomponent.init(settings);

var element = document.getElementById("Container"); //Scope the bindings to one container element

ko.applyBindings(viewModel, element);
```

## Date handling

**momentjs** used for Parse, validate, manipulate, and display dates in JavaScript.
[http://momentjs.com/](http://momentjs.com/)

**momentjs timezone** used for Parse and display dates in any timezone.
[http://momentjs.com/timezone/](http://momentjs.com/timezone/)
 
## Currency handling

**accounting.js** is a tiny JavaScript library, providing simple and advanced number, money and currency formatting.
Features custom output formats, parsing/unformatting of numbers, easy localisation and spreadsheet-style column formatting (to line up symbols and decimals).
It's lightweight, has no dependencies and is suitable for all client-side and server-side JavaScript applications

[http://openexchangerates.github.io/accounting.js/](http://openexchangerates.github.io/accounting.js/)