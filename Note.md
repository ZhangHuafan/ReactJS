# ReactJS: INST

## Basic JS
- Primitives: string, number, boolean, null, undefined
  - having no properties / methods
  - immutable (hardcoded)
- Objects: a collection of names values; variables containing single or many values
  - many: `var person = {firstName:"John", lastName:"Doe", age:50};`
    - ***name:value*** pair (similar to HashMap in Java)
  - properties: named value (*firstName*)
  - object method: an object property containing a function definition
  - mutable
- `setInterval(function, milliseconds,)` method: calls a function or evaluates an expression at specified intervals (in milliseconds); will continue executing util:
  - `clearInterval(id)` is called: use id value returned by `setInterval()`
  - the window is closed
- Const
  - `const`: define a constant ***reference*** to a value
    - must be assigned a value when declared
    - the constant primitive values cannot be changed
    - the properties of constant objects can be changed (`.`operator)
  - let: similar to `const` but:
    - can be reassigned
    - no need to be assigned when declared
- `Number.isNaN(input)`
- comparison:
  - `==`: converting the operands to the same type first
  - `===` some type & matching content

## Main Concepts

### Intro to JSX
- Use JSX with React to describe the appearance of UI (rcm)
  - wrap var inside JSX: `const element = <h1>Hello, {var}</h1>;`
  - put valid JS exp. inside `{}`: *2+2, function...*
- JSX => JS objects: can be used in assignment, arguments, and return value
- Specify attributes using
  - `""` for literals
  - `{}` for JS exp.: `const element = <img src={user.avatarUrl}></img>;`
    - No `""`
- `React.createElement()`:
```JS
const element = React.createElement(
    'h1',
    {className: 'greeting'},
    'Hello, world!'
);
```
- Use ***Babel*** language definition for the editor to highlight both ES6 & JSX

### Rendering Elements
- Def.
  - Element: smallest building blocks of React app
    - immutable: cannot change its children / attributes => for updating: create a new & render again
  - root DOM node: everything inside it managed by React DOM
    - single one for building with React; many isolated ones for integrating React into existing apps
- ReactDOM.render(element, root): render a React element into a root DOM node
  - root might be obtained by: `document.getElementById('root')`

### Components and Porps
- Def.
  - components: conceptually similar to JS functions
    - accept ***props*** *(properties)* as arbitrary inputs
    - return React elements
    - e.g.: button, form , dialog
- define a component:
  - write a JS function => function component
  ```JS
  function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
  }
  ```
  - use an ES6 class: `extends React.Component`
  ```JS
  class Welcome extends React.Component {
      render() {
          return <h1>Hello, {this.props.name}</h1>;
      }
  }
  ```
- use element to represent user-define components:  `const element = <Welcome name="Sara" />;`
  - pass JSX attributes (*name*) to this component (*Welcome*) as props
- Structure:
  - start new app: a single ***App*** component at the top
  - integrate: bottom-up with small components: *Button*
- Props are read-only: All React components must act like pure functions
  - pure function:
    - not changing the inputs
    - always return the same result given the same inputs
    - <=> impure

### State and Lifecycle
- Def.
  - State: similar to props except:
    - private
    - fully controlled by the component
    - differences between ***prop***:
      - prop: to be passed; similar to function parameters
      - state: to be managed within the component; similar to variables declared within a function
- Convert function component to class:
  - extends `React.Component`
  - move function body to `render()` method
    - `render()` will be called for each update while only one class instance is used
  - use this to access Props
- Add local state to class *example about Clock* :
  - change this.props.date -> this.state.date
  - add class constructor:
  ```JS
  constructor(props) {
      super(props);
      this.state = {date: new Date()};
  }
  ```
    - rmb to call the base constructor with props
  - remove `date` property from `<Clock />` element

- Add Lifecycle methods:
  - mount: set up a *timer* when *Clock* is rendered for the first time => `componentDidMount(){}`: runs after the component output has been rendered to DOM
  - unmount: remove the *timer whenever DOM of *Clock* is removed => `componentWillUnmount(){}`

- properties of `this`:
  - this.props: set up by React
  - this.state: hold the changing data
    - `this.setSate()``: schedule updates to `state`
  - other fields can be added manually to store something: using `this.xxx = ...` directly

- Reminders about using state:
  - use `setSate()` instead of modifying state directly such as `this.state.comment = 'Hello'`
    - only do assignment in constructor
  - do not rely values of props and state for calculating next state as the updates are ***asynchronous***
   - use setSate(function)
  - can use multiple `this.setState()` to update different class members
- Pass state down: just pass A's state down as props of B
  - Unidirectional data flow: component tree as a waterfall of prpps, and states are additional water source to components below them

### Handling Events
- differences between DOM event handling:
  - events are camelCase
  - with JSX, pass a function instead of string like `activateLasers()`:
  ```JS
  <button onClick={activateLasers}>
      Activate Lasers
  </button>
  ```
  - must call `preventDefault` explicitly to prevent default behavior
  - provide a ***listener*** when the element is initially rendered instead of after its creation
- Common pattern using ES6 class: make an event handler to be a method
  - binding is necessary to make `this` work in the callback: `this.handleClick = this.handleClick.bind(this)`
    - class methods are not bound by default => `this` will be undefined
    - generally, when referring to methods without `()`, should bind that method
  - ways other than binding:
    - use class fields to correctly bind callbacks
    - use arrow function (nrmd)
- Pass arguments like *id*: `<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>`
  - `e` representing the React event will be passes as a second argument after `id` automatically
- Full example:
```JS
class Toggle extends React.Component {
    constructor(props) {
      super(props);
      this.state = {isToggleOn: true};
      this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
      this.setState(state => ({
        isToggleOn: !state.isToggleOn
      }));
    }

    render() {
      return (
        <button onClick={this.handleClick}>
          {this.state.isToggleOn ? 'ON' : 'OFF'}
        </button>
      );
    }
}
```
### Conditional Rendering
- example: greeting user or guest based on whether a use is logged in
- different syntax for condition:
  - `if{} else{}`
  - inline `if` with `&&`
    - okay for single exp. as condition: `{unreadMessages.length > 0 && ...(other DOM elements)`
    - can embed any exp. by wrapping them in `{}`
  - inline if-else: `condition ? true : false`
- Prevent (Hide) component from rendering: return `null`
  - doe snot affect LCMs

### Lists and Keys
- example:
```JS
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
    <li>{number}</li>
);
```
- Usually render lists inside a component
- Keys: provides identity for items
  - use a string that uniquely identifies a list item among its siblings
  ```JS
  const todoItems = todos.map((todo) =>
    <li key={todo.id}>
      {todo.text}
    </li>
  );
  ```
  - only use index if items have no stable IDs (*default*)
- Cautions:
  - extract components with keys: should keep the key on elements in the ***array*** rather than on `<li>` of the extracted component
    - ***Elements inside `map()` need keys***
  - be unique within that array (not globally)

### Forms
- controlled component: an input form element whose value is controlled by React
  - in the way that React component renders a form also & controls user input in that form
- code example (partial):
```JS
  render():
  <input type="text" value={this.state.value} onChange={this.handleChange} />

  class constructor:
  this.state = {value: ''};

  class method:
  handleChange(event) {
    this.setState({value: event.target.value});
  }
```
  - `handleChange(event)` runs on every keystroke to update the React state => easy to modify or validate user input by adding additional controls in `handleChange(event)`
- `<textarea>`: uses a value attribute; can be written similarly to above with `this.state` initialized to `{value: 'some info'}`
- `<select>`: similar to above with using `value to indicate initially selected tag`
```JS
<select value={this.state.value} onChange={this.handleChange}>
    <option value="grapefruit">Grapefruit</option>
    <option value="lime">Lime</option>
    <option value="coconut">Coconut</option>
    <option value="mango">Mango</option>
</select>
```
  - `<select multiple={true} value={['B', 'C']}>`: allow to select multiple options
- Handle multiple inputs:
  - add `name` attribute to each element
  - let handler function choose what to do based on `event.target.name`
  ```JS
  this.setState({
      [name]: value
  });
  ```
    - computed property name: `[exp.]` to be used as property name
- [Formik](https://jaredpalmer.com/formik)

### Lifting State Up
- lifting state up: move it up to the closest common ancestor for sharing state
- Detailed steps (*example of Temperature*):
 - in `TemperatureInput`, change `this.state.temperature` to `this.props.temperature`
  - reminder: props are read-only
 - let the parent provide both `temperature` and `onTemperatureChange`
  - the parent should have necessary properties: `temperature`, `scale`
 - [complete code](https://reactjs.org/docs/lifting-state-up.html)
- reminder: a single source of truth for all data change

### Composition vs Inheritance
- Use composition instead of inheritance (rcm)
- containment_code examples:
```JS
function FancyBorder(props) {
    return (
      <div className={'FancyBorder FancyBorder-' + props.color}>
        {props.children}
      </div>
    );
}
```
  - children are unknown ahead of time => use `children` prop to pass children elements directly into their output
  - multiple holes: pass concrete elements to certain prop such as `left`, `right`... (not limited to `children`)
- Specialization: a more *specific* component renders a more *generic* one and configures it with props
```JS
function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```
 - work for class components as well; just write concrete elements in `render()` method
- reuse non-UI functionality between components:
  - extract them into a separate JS module
  - import and then use

### Thinking in React
- Steps of building:
  - Based on mock ui, draw boxes to identify each component => arrange them into a hierarchy
  - Build a static version: do not use state at all (iguws)
    - top-down: simple project
    - bottom-up: large project
  - identify the minimal representation of UI state: ***DRY***
    - not use state if:
      - it is passed in from a parent via props
      - it remain unchanged over time
      - it can be computed based on other state / prop
  - Identify which component owns the state
    - identify every component rendering something based on that states
    - find a single component above all these components
    - either this component or its parent should own it => if not suitable to own, create a new one solely for holding the state & add it in the hierarchy
  - add inverse data flow: add handlers in children class (which are concretely defined in the parent holding state)
  ```JS
  handleFilterTextChange(e) {
      this.props.onFilterTextChange(e.target.value);
  }
  ```
    - remember to bind method to `this`: `this.handleFilterTextChange = this.handleFilterTextChange.bind(this);`


## Advanced Guides

## Coding Standard
- split JSX over multiple lines; similar to HTML
- wrap HTML tags inside `()`
- use camelCase property naming convention
  - e.g.: `className` as HTML's `class`
- Start component (*Welcome*) names with a CAPITAL letter
  - the component (*Welcome*) will be required in scope
  - starting with lowercase => treated as DOM tags
- Name props from the component's view instead of the context
- Extract components to increase reusability
  - when `map()` body is too nested
 -

## Other Info
- JSX prevents injection attack (XSS): everything is converted to a string before being rendered
- [React Developer Tools](https://github.com/facebook/react/tree/master/packages/react-devtools)
