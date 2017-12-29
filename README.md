## Introduction
In previous blog, we have learned the fundamentals of react including React concepts, JSX syntax, concept of components, and a HMR react dev environment set-up. In this blog, we will dig more into react - an important point - state management.
## Table of Contents
1. State Intro
2. Pass Data with Props
3. Functional Components
4. Add State to A component
5. Update state with setState
6. PropTypes
7. Controlled Component
8. Lesson Summary
## Content
### 1. State Intro
In the blog, we will introduce three new concepts:
  1. Props \- Allow you to pass data into your components
  2. Functional Components \- An alternative way to create components
  3. Controlled Components \- Allow you to hook up the forms in your application to your component state

By introducing those concepts, we are going to build an react app that will list your contacts, search the contact you want, and add new contacts.
Finished project can be found here.
https://github.com/BridgeNB/React-Learning-Note-2.git
### 2. Pass Data with Props
Given an example
```jsx
/* Use props we can access the value from inside of component*/
class User extends Component {
  render() {
    return (
      <div>
        <p> Username: {this.props.username} </p>
        <p> Is Friend?: {this.props.friend} </p>
      </div>
    )
  }
}
<User username='Tyler' friend={true} />
```
Given another example
```jsx
/* In this example, we pass contact to listContacts component. The reason we need an unique key attribute on every list iterm is due to performace concern. If react knows which item has changed in the list, it doest need to upgrade the whole list.*/
class ListContacts extends Component {
  render() {
    return (
      <ol className='contact-list'>
        {this.props.contacts.map((contact) => (
            <li key={contact.id}>
              {contact.name}
            </li>
        ))}
      </ol>
    )
  }
}
```
Further expand
```jsx
/* We can even expand more to pass avatar, contact.name, contact.email to listContact component*/
class ListContacts extends Component {
  render() {
    return (
      <ol className='contact-list'>
        {this.props.contacts.map((contact) => (
            <li key={contact.id} className='contact-list-item'>
              <div className='contact-avatar' style={{
                backgroundImage: `url(${contact.avatarURL})`
              }}/>
              <div className='contact-details'>
                <p>{contact.name}</p>
                <p>{contact.email}</p>
              </div>
              <button className='contact-remove'>
                Remove
              </button>
            </li>
        ))}
      </ol>
    )
  }
}
/* `` the back ticks allow for embedded expression. Using this kind of template literals, you can interpolate expressions as normal strings.*/
```
Recap:
A prop is any input that you pass to a React component. Like HTML attribute, a prop name and value are added to the component.
```html
<Button text="This is a button" />
```
In this case, text is a prop and the string This is a button is the value.
Remember, all the props are stored on this.props object. So in order to access text prop from inside the component, we'd use this.props.text with curly braces.
```jax
render() {
  return <div>{this.props.text}</div>
}
```
### 3. Functional Components
stateless functional componenets
```jsx
/* if the componenet only has render method, we can build a stateless functional components */
function ListContacts (props) {
    return (
      <ol className='contact-list'>
        {props.contacts.map((contact) => (
            <li key={contact.id} className='contact-list-item'>
              <div className='contact-avatar' style={{
                backgroundImage: `url(${contact.avatarURL})`
              }}/>
              <div className='contact-details'>
                <p>{contact.name}</p>
                <p>{contact.email}</p>
              </div>
              <button className='contact-remove'>
                Remove
              </button>
            </li>
        ))}
      </ol>
    )
}
/*Above code is equivalent to previous code snippet*/
```
#### Recap
```jsx
class Book extends React.Component {
  render() {
    return (
      <div>
        {this.props.text}
      </div>
    );
  }
};
const Book = (props) => (
  <div>
    {props.text}
  </div>
);
/* They are equivalent. The second called stateless functional component */
```
### 4. Add State to A component
#### Intro
Although props refer to attributes from parent component, it only represents read-only data that are immutatble. A component state, on the other hand, represents mutable data taht ultimately affects the finally render effection.
Given an example:
```jsx
class User extends React.Component {
  state = {
    username: 'Bridge'
  }
  render () {
    return (
      <p> Username: {this.state.username} </p>
    )
  }
}
/* In more formal way */
class User extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      username: 'Bridge'
    }
  }
}
```
#### Recap:
React will know and make the necessary updates by having a component manage its own state. So we just need to think about updating state without concerned others, such as render performance. It is a key advantage of React.
### 5. Update state with setState
React cannot allow to change state directly because it will cause confusion. React use <strong>setState</strong> to change state. There are two ways to use setState and examples are given below:
```jsx
/* 1. Use setState directly as a funtion. Use this case when you want to udpate state based on previous state */
this.setState((prevState) => ({
  count: prevState.count + 1
}))
/* 2. Use setState as an object. Use this everytime except the first case */
this.setState({
  username: 'Bridge'
})
```
#### Recap
Use this.setState() to merge state updates or to update a state based on previous state.
### 6. PropTypes
Using proptype can reduce amount of time on debugging. It will specify the specific types of each prop we are going to pass and type being, such as string, array, object, function, things like that, and it will also say those parameters required or not.
Given an example
```jsx
/* After import proptypes, we can use prop types to ensure the parameters in ListContacts are array and func and both of them are required */
import PropTypes from 'prop-types'
ListContacts.prototype = {
  contacts: PropTypes.array.isRequired,
  onDeleteContact: PropTypes.func.isRequired
}
```
#### Recap
Proptypes are great way to validate intended data types in React app. It can check bot datatypes and requirments to reduce debugging time.
### 7. Controlled Component
Controlled Components which render a form, but the source of truth for that form state lives inside of the component state rather than inside of DOM.
Given an example:
```jsx
class NameForm extends React.Component {
  state = { email: ''}
  handleChange = (event) => {
    this.setState({email: event.target.value});
  }
  render() {
    <form>
    <input type='text' value={this.state.email} onChange={this.handleChange} \>
    </form>
  }
}
```
More specific example can be viewed in ListContacts.js file, we used a search bar with instant result
```jsx
  <div className='list-contacts-top'>
    <input
      className='search-contacts'
      type='text'
      placeholder='Search contacts'
      value={this.state.query}
      onChange={(event) => this.updateQuery(event.target.value)}
    />
  </div>
  /* every time anyting change on the query we will update the text in the search bar */
```
<strong>Advantages</strong>
1. Instant input validation
2. Conditionally disable / enable form buttoms
3. enforce input formats
