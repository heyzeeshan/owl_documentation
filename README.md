# Odoo Owl Framework Documentation

# 1.Introduction
The Odoo Web Library (OWL) is a relatively small UI framework intended to be the basis for the Odoo Web Client in future versions (>15). Owl is a modern framework, written in Typescript, taking the best ideas from React and Vue in a simple and consistent way. 

# 2.Components
Components are JavaScript classes with properties, functions and the ability to render themselves (Insert or Update themselves into the HTML Dom). Each Component has a template that represents its final HTML structure, with composition, we can call other components with their tag name inside our Component.

```class MagicButton extends Component {
  static template = xml`
    <button t-on-click="changeText">
      Click Me! [<t t-esc="state.value"/>]
    </button>`;

  state = { value: 0 };

  changeText() {
    this.state.value = "This is Magic";
    this.render();
  }
}
```
The templating system is in XML QWeb, which should be familiar if you are an Odoo Developer. t-on-click allow us to listen to the click event on the button and trigger a function defined inside the Component called changeText. 
Properties of the Component live inside the state property, it is an object that has all the keys/value we need. This state is isolated and only lives inside that Component, it is not shared with other Components (even if they are copies of that one).
Inside that changeText function we change the state.value to update the text, then we call render to force the update of the Component display: the Button shown in the Browser now has the text "Click Me! This is Magic".

# 3.Hooks and reactivity
It is not very convenient to use render function all the time and to handle reactivity better, OWL uses a system its system of hooks, specifically the useState hook.

```const { useState } = owl.hooks;

class MagicButton extends Component {
  static template = xml`
    <button t-on-click="changeText">
      Click Me! [<t t-esc="state.value"/>]
    </button>`;

  state = useState({ value: 0 });

  changeText() {
    this.state.value = "This is Magic";
  }
}
```

As we can see, we don't have to call the render function anymore. Using the useState hook actually tells the OWL Observer to watch for change inside the state via the native Proxy Object.

# 4.Passing data from Parent to Child via props
We saw that a Component can have multiple Components inside itself. With this Parent/Child hierarchy, data can be passed via props. For example, if we wanted the initial text "Click me" of our MagicButton to be dynamic and chosen from the Parent we can modify it like that

```const { useState } = owl.hooks;
class MagicButton extends Component {
  static template = xml`
    <button t-on-click="changeText">
      <t t-esc="props.initialText"/> [<t t-esc="state.value"/>]
    </button>`;

  state = useState({ value: 0 });

  changeText() {
    this.state.value = "This is Magic";
  }}
// And then inside a parent Componentclass Parent extends Component {
  static template = xml`
<div>
    <MagicButton initialText="Dont click me!"/>
</div>`;
  static components = { MagicButton };
  ```

# 5.Odoo OWL utils patch, and patchMixin functions.

# 6.Usefull Links and examples
i.https://github.com/odoo/owl - github unofficial doc

ii.https://github.com/odoo/owl/blob/master/doc/learning/overview.md - unoffical doc

iii.https://codingdodo.com/realworld-app-with-owl-odoo-web-library-part-1/ - (Use of starter template, general knowledge, Components architecture, and Routing)

iv.https://codingdodo.com/realworld-app-with-owl-odoo-web-library-part-2/ - (OWL Store for State managements, authentication, Routing guards, APIRequests)

v.https://codingdodo.com/realworld-app-with-owl-odoo-web-library-part-3/ - (Dynamic Routing, async components functions willUpdate and willUpdateProps).

vi.https://codingdodo.com/realworld-app-with-owl-odoo-web-library-part-4/ - (Cleanup and refactoring into  Hooks).

vii.https://codingdodo.com/owl-in-odoo-14-extend-and-patch-existing-owl-components/ - And this article, is focused on OWL inside Odoo 14 

viii.https://owl-realworld.netlify.app/#/ - example
