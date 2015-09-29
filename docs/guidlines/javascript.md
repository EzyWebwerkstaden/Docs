### Javascript

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
