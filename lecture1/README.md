# React basics for beginners

This tutorial is meant for developers interested in learning React, and was written for the React.JS for Beginners course in NYC in August 2015. 

It doesn't assumes that know any React beforehand, but you should be familiar with Javascript.

## Components

React is built around components, not templates. You create a component by calling the createClass method on the React object, which is the entry point into the library. Like this:


	var ButtonApp = React.createClass({
		render: function(){
			return (
				<input type="submit" />
			);
		}
	});

The createClass method takes one argument, which is called the specification object. This object contains all the methods for the given component class. The most important one is the render() function, which is triggered when the component is ready to be drawn on the page.  

So within it, you'll return whatever you want React to render on the page, naturally. In the case above, we simply want to render a button.   

Note: The return statement in the render function is a description of the UI at **any given time.** So if the data within it changes, React will take care of updating the UI accordingly.


## JSX

The HTML'ish syntax within the render function is not actually HTML, but something called JSX. This is simply a syntax extension for Javascript which enables you to write JS with XML-like tags. So the tags are actually function calls, and not HTML. But for now, you can simply think of it as just HTML/XML with a few extra abilities.  

## Multiple components

If you want to nest multiple components together, you do this within the return statement in the render function, like we're doing below, where we're nesting  **ButtonForm** within the **App** component. At this point the **App** component owns the **ButtonForm** component. It's the type of parent-child relationship you probably recognize from HTML.

	var ButtonForm = React.createClass({
		render: function(){
			return (
				<div>
					<h3>Click the button</h3>
					<input type="submit" />
				</div>
			);
		}
	});

	var App = React.createClass({
		render: function(){
			return (
				<div>
					<h1> Welcome to my button app!</h1>
					<ButtonForm />
				</div>
			);
		}
	});
	
	React.render(<App />,  document.getElementById("content"));

The *React.render()* function you see below the two components takes care of 'kickstarting' the rendering, and renders the root component (the common ancestor for all the components) into the DOM in the specified container. 

## Props & State

There are two types of data in React; state and props. The difference between the two is a bit tricky to understand in the beginning, at least conseptually. But once you start working with the library, you'll quickly manage to separate the two from each other.  

The key difference is that state is private and controlled from within the component itself. Props are external and controlled by whatever renders the component. So a component can not change its own props directly, only indirectly. A component can, however, change its own state. As a beginner, you can think of state as dynamic data and props as static data, at least from the perspective of the within the components, even though that's bit too simplified, as the props do change all the time.

### Props  

Let's start off by looking a bit closer on props, as it forces us to understand Reacts one directional data flow. 

Lets have a look at our little button app and initialize it with some data, using props. Firstly we'll need to grab the data from somewhere. This could for example be done using an Ajax call to fetch some data from an API, but for now we'll just hard code it in as a variable:
	
	var BUTTONTEXT = "Click the button";

The way to hand this data to a components props looks a lot like how you would specify an HTML element's attribute.

	<App text={BUTTONTEXT} />

Once the **App** component is initialized like this, it can access the BUTTONTEXT variable through *this.props.text*. Let's see it in action:

		var BUTTONTEXT = "Click the damn button";

		var ButtonForm = React.createClass({
			render: function(){
				return (
					<div>
						<h3>{this.props.text}</h3>
						<input type="submit" />
					</div>
				);
			}
		});

		var App = React.createClass({
			render: function(){
				return (
					<div>
						<h1> Welcome to my button app!</h1>
						<ButtonForm text={this.props.text} />
					</div>
				);
			}
		});
		
		React.render(<App text={BUTTONTEXT} />,  document.getElementById("content"));

You'll see the props is being passed into the App component in the React.render() function.

PS: The reason we're wrapping the **BUTTONTEXT** in curly braces it because we'll need tell the JSX that we want to add a Javascript expression.   

In addition to accessing the BUTTONTEXT variable through this.props.text, the **App** component can also pass the data down to its own children, as it does. It initializes the ButtonForm component with the same props it got itself; we're saying that passes it down the chain.

When the data reaches the ButtonForm, it's found its destination, as it's rendered as the descrption text above the button.

This way of passing props - from parent to child - is how data is passed around in React apps. It's passed down the chain, and it's passed as props.  

### State 

The other way of storing data in React is in the component’s state. If you want the data to change - for example based on user interactions - it most likely has to be stored in a component's state somewhere in the app, in what we call a stateful component.

Hence, state is what you use when you want to update the UI; every time the UI changes, it's ultimately a consequence of one component's changing its state.

As mentioned previously, state is private and owned by one component; the state is never passed down the chain. If you want to pass the data on to a child of the component it **must be done using props.**

Another difference betweeen the two is that you don't initialize state through JSX, as you do with props. You do it through **getInitialState()**instead.

Also, a component can change it's own state, while it can't change it's own props (at least not directly)- The way a component changes the state is by **this.setState().** The following example illustrates the point:

	var App = React.createClass({

		getInitialState: function(){
			return {
				active: true
			}
		},

		handleClick: function(){
			this.setState({
				active: !this.state.active
			});
		},
		
		render: function(){
			var buttonSwitch = this.state.active ? "On" : "Off";

			return (
				<div>
					<p>Click the button!</p>
					<input type="submit" onClick={this.handleClick} />
					<p>{buttonSwitch}</p>
				</div>
			);
		}
	});

	React.render(<App />,  document.getElementById("content"));






