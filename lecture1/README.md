# React basics for beginners

This article is the basis for the first lecture in the React.JS for Beginners course in NYC in August 2015. 

It doesn't assume that know any React beforehand, but you should be comfortable with Javascript.

## Your first component

React is built around components, not templates. You create a component by calling the *createClass* method on the React object, which is the entry point into the library. Like this:


	var ButtonApp = React.createClass({
		render: function(){
			return (
				<input type="submit" />
			);
		}
	});

The *createClass* method takes one argument: the specification object. This object contains all the methods for the given component class. The most important one is the **render()** method, which is triggered when the component is ready to be drawn on the page.  

Within it, you'll return a description of what you want React to render on the page. In the case above, we simply want it to render a button.   

Note: The render function is a description of the UI at **any given time.** So if the data within it changes, React will take care of updating the UI accordingly.


## JSX - Javascript Syntax Extension

The HTML'ish syntax is not actually HTML, but something called JSX. This is simply a syntax extension for Javascript which enables you to write JS with XML-like tags. So the tags are actually function calls, which are transformed into React.JS code, and finally end up as HTML and Javascript in the DOM.  

But for now, you can simply think of it as just HTML/XML with a few extra abilities.   

Note: You can write JSX in both .js and .jsx files, but you have to transform it from JSX to React.JS with a transpiler or pre-processor. You'll also need to mark you script tag with text/jsx instead of text/js. Like this:

	<script type="text/jsx" src="main.js">


## Multiple components

If you want to nest multiple components together, you do this within the return statement of the render function, like we're doing below, where we're nesting  **ButtonForm** within the **App** component. At this point the **App** component owns the **ButtonForm** component. It's the type of parent-child relationship you probably recognize from HTML.

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

The **React.render()** method you see below the two components takes care of 'kickstarting' the rendering, and renders the root component (in this case **App**) into the DOM in the specified container. 

A good habit is to create a mockup of your UI before you start coding, and separate the components with colors. Below is an example of how a component structure in React could look like visually, for a simple Todo app:

![image](https://github.com/ReactForBeginners/exercise1-todo/blob/gh-pages/todo.png?raw=true)

## Props & State

There are two types of data in React; props and state. The difference between the two is a bit tricky to understand in the beginning, at least conceptually. But once you start coding, you'll quickly manage to separate the two from each other.  

The key difference is that state is private and can be changed from within the component itself. Props are external, and not controlled by the component itself. It's passed down from components higher up the hireachy, whom also control the data.  

So while a component can not change its own props directly (it can do it indirectly, but let's save that for later), it can change its own state. 

### Props  

We'll start off by looking a bit closer on props, as it forces us to understand Reacts one directional data flow, which also is critical to know about.

Let's initialize our button app with some data, using props. First we'll need to grab the data from somewhere. This could for example be done using an Ajax call to fetch some data from an API, but for now we'll just hard code it as a variable:
	
	var BUTTONTEXT = "Click the button";

The way to hand this data to a component's props looks a lot like how you would specify an HTML element's attribute.

	<App text={BUTTONTEXT} />

The reason we're wrapping the BUTTONTEXT in curly braces it because we'll need tell the JSX that we want to add a Javascript expression.

Once the **App** component is initialized like this, it can access the BUTTONTEXT variable through *this.props.text*. However, it can not change the data directly. From the components perspective, its props are immutable. Its just something its initialized with.

Here's an example:

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

The props are being passed into the **App** component in **React.render()**. The **App** component can now access the **BUTTONTEXT** variable through *this.props.text*; it can also pass the data down to its own children, as it does. It initializes the **ButtonForm** component with the same props it got itself; we're simply passing the data down the chain.

When the data reaches the **ButtonForm** component, it's found its destination, as it's rendered as the descrption text in the **h3**-tag above the button.

This way of passing props down the chain - from parent to child - is how data is distributed in React. It's passed down the hirearchy, and it's passed as props.  

### State 

The other way of storing data in React is in the component’s state. And unlike props - which are immutable from the components perspective -  the state is mutable.  

So if you want the data in your app to change - for example based on user interactions - it must be stored in a component's state somewhere in the app.  

As state is private and owned by one component only, it can't be passed down the chain to child components. If you want to pass the data down to a child, you'll have to pass is as a props.  

**Initializing state**

To initialize the state simply pass a **getInitialState()** to the component, and return whatever state you want your component to begin with.

**Changing state**

To modify the state, simply call **this.setState(),** passing in the new state as the argument.  

An example:

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


This example also force you to get familiar with the Reacts event system, but don't worry, it's very simple. We hook an event listener onto the button, listening for the **onClick** event. When this is triggered, we call the **handleClick** function, which is available through the **this** keyword. 

The **handleClick** function then calls **this.setState()** which toggle the **active** variable between true & false.

Note: Reacts [events](https://facebook.github.io/react/docs/events.html) are synthetic; they are cross browser wrappers around the browser's native events. So React makes sure that your chosen event works identically across all browsers.

## Where should the state live?

In general, you should try and keep as few stateful components as possible, and also store as little data as possible in the state. If your stateless components need access to the data from the state, simply pass the data down to them as props.

To figure out where the state should live, you can ask yourself there questions, pulled from the original [React docs:](https://facebook.github.io/react/docs/thinking-in-react.html#step-4-identify-where-your-state-should-live)

* Identify every component that renders something based on that state.
* Find a common owner component (a single component above all the components that need the state in the hierarchy).

* Either the common owner or another component higher up in the hierarchy should own the state.

* If you can't find a component where it makes sense to own the state, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component.


## Inverse data flow

We've talked a lot about how data only flows one way in React; from parent to child. That's not entirely true, as it's possible to add an inverse data flow.

You'll need this when a component deep into the hirearchy need to change its parent's state.  

Below is an example of how a buttonclick in the **ButtonForm** component trigger a state change in the **App** component above it, as it can access the **onUserClick** function. 
	
	var ButtonForm = React.createClass({
		render: function(){
			return (
				<div>
					<input type="submit" onClick={this.props.onUserClick} />
					<h3>You have pressed the button {this.props.counter} times!</h3>
				</div>
			);
		}
	});

	var App = React.createClass({
		getInitialState: function(){
			return {
				counter: 0
			}
		},
		onUserClick: function(){
			var newCount = this.state.counter += 1;
			this.setState({
				counter: newCount
			});
		},
		render: function(){
			return (
				<div>
					<h1> Welcome to the counter app!</h1>
					<ButtonForm counter={this.state.counter} onUserClick={this.onUserClick} />
				</div>
			);
		}
	});

	React.render(<App />,  document.getElementById("content"));

As you can see, we're simply passing down the onUserClick method as a props, enabling the **ButtonForm** component to *reach up* to the **App** component, and trigger one of its methods.

# refs and findDOMNode

Sometimes you might want to reach into the DOM and do some changes, but not necessarily involve the state and props. In these situations, you'll need to grab the node of your choice.  

Luckily React provides a handy way of grabbing DOM nodes. Simply call **React.findDOMNode(component)**, passing in the component of your choice.

In order to get a reference your chosen component, you can use the *refs* attribute. Simply add a *ref* to a component like this:

	<input ref="textField" ... />

From within the component rendering the input tag above, you can reference the input field through *this.refs.textField*.  

Let's look at a proper example:

		var ButtonForm = React.createClass({
			focusOnField: function(){
				React.findDOMNode(this.refs.textField).focus();
			},
			render: function(){
				return (
					<div>
						<input 
							type="text"
							ref="textField" />
						<input 
							type="submit"
							value="Focus on the input!" 
							onClick={this.focusOnField} />
					</div>
				);
			}
		});

		var App = React.createClass({
			render: function(){
				return (
					<div>
						<h1> Welcome to the focus app!</h1>
						<ButtonForm onUserClick={this.onUserClick} />
					</div>
				);
			}
		});

		React.render(<App />,  document.getElementById("content"));

This will result in the input text field being focused on when you click the button.

## key

When you're creating components dynamically, each of them need a unique "key" attribute. Like this:  

	var App = React.createClass({
		getInitialState: function(){
			return {
				todos: ["get food","drive home","eat food", "sleep"] 
			}
		},

		render: function(){
			var todos = this.state.todos.map(function(todo,index){
				return <li key={index}>{todo}</li>
			});				
			return (
				<div>
					<h1> Welcome to the ToDo list!</h1>
					<ul>
						{todos}		
					</ul>
				</div>
			);
		}
	});

