# React basics for beginners

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

Note: The render function is a description of the UI at **any given time.** So if the data within the return statement in the function changes, React will take care of updateing the UI accordingly.


## JSX

The HTML'ish syntax within the render function is not actually HTML, but something called JSX. This is simply a syntax extension for Javascript which enables you to write JS with XML-like tags. So the tags are actually function calls, but for now you can simply think of it as just HTML/XML with a few extra abilities.  

## Multiple components

If you want to nest multiple components together, you do this within the return statement of the render function, like we're doing below, where we're nesting the. At this point the **App** component owns the **ButtonForm** component. It's a parent-child relationship you probably recognize from HTML.

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

The *React.render()* function you see below the two components takes care of 'kickstarting' the rendering, and renders the root component (the common ancestor for all the compoents). It simply renders this component into the DOM in the specified container. 

## Props & State

There are two types of data in React; state and props. The difference between the two is a bit tricky to understand in the beginning, at least conseptually. But once you start working with the library, you'll quickly manage to separate the two.  

The key difference is that state is private and controlled from within the component itself. Props are external and controlled by whatever renders the component. So a component can not change its own props directly, only indirectly. A component can, however, change its own state. As a beginner, you can think of state as dynamic data and props as static data, even though that's bit too simplified.  

**Props**  

Let's start off by looking a bit closer on props, as it forces us to understand Reacts one directional data flow. Below is a component which is initialized with some props.

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

The props is being passed into the App component in the React.render() function at the bottom. This is how you initialize a component with props; it similar to how you'd add attributes to an HTML tag.  

PS: The reason we're wrapping the **BUTTONTEXT** in curly braces it because we'll need tell the JSX that we want to add a Javascript expression.   

Now, the App component can access this data via this.props.text, and it can also pass the data down to its own children, as it also does. It initializes the ButtonForm component with the same props it has got itself; passes it down the chain.

ButtonForm on the other hand, use the props as a descrption text above the button.

This way of passing props - from parent to child  - is how data is passed around in React apps. It's passed down the chain; always as props.  

**State** 
