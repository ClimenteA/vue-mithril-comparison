# vue-mithril-comparison
A comparison between vue and mithril in the base of vue docs.

See src/App.js for the code


```javascript


// Declarative-Rendering

/*

<div id="app">
  {{ message }}
</div>

var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})

*/


const HelloWorld = {
    view: v => [
        m("h1", `Hello ${v.attrs.message}`)
    ]
}

m.render(
    document.getElementById("app-1"), 
    m(HelloWorld, {message: "world"})
)



// Attributes interpolation

/*

<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>

var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})


*/


const AttrsInterpolation = {
    view: v => [
        m("span", {title: v.attrs.message}, 
        "Hover your mouse over me for a \
        few seconds to see my dynamically bound title!")
    ]
}


m.render(
    document.getElementById("app-2"),     
    m(AttrsInterpolation, 
        {message: 'You loaded this page on ' + new Date().toLocaleString()}
    )
)



// Conditionals and loops


/*

<div id="app-3">
  <span v-if="seen">Now you see me</span>
</div>

var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})

*/


const ConditionalSeen = {
    view: v => [
        v.attrs.seen ? m("span", "Now you see me") : null  
    ]
}


m.render(
    document.getElementById("app-3"),     
    m(ConditionalSeen, {seen: true})
)



// Loops

/*

<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>

var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})

*/


const LoopObj = {
    view: v => m("ol", v.attrs.todos.map(todo => {
        return m("li", todo.text)
    }))
}


m.render(
    document.getElementById("app-4"),     
    m(LoopObj, {

        todos: [
            { text: 'Learn JavaScript' },
            { text: 'Learn Vue' },
            { text: 'Build something awesome' }
        ]
    })
)




// Methods

/*


<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>


var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})

*/


const ReverseMsg = () => {

    let message = "Hello Mithril.js!"
    
    function reverseMessage() {
        message = message.split('').reverse().join('')
    }

    return {
        view: v => [
            m("p", message),
            m("button", {onclick: reverseMessage}, "Reverse Message")
        ]
    }
}


m.mount(
    document.getElementById("app-5"),     
    ReverseMsg()
)



// Two-way binding

/*

<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>

var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})

*/


const DuplexBind = () => {

    let message = "Hello Mithril.js!"
    
    function bindText(event) {
        message = event.target.value
    }

    return {
        view: v => [
            m("p", message),
            m("input", {
                value:message, 
                oninput: e => bindText(e)
            })
        ]
    }
}


m.mount(
    document.getElementById("app-6"),     
    DuplexBind()
)





// Composing-with-Components

/*

<div id="app-7">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})

*/



const TodoItem = {
    view: v => v.attrs.groceryList.map(grocery => {
        return m("li", {key: grocery.id}, grocery.text)
    })
}


function OrderedList() {
    
    let groceryList = [
        { id: 0, text: 'Vegetables' },
        { id: 1, text: 'Cheese' },
        { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
    
    return {
        view: v => m("ol", m(TodoItem, {groceryList}))
    }
}


m.mount(
    document.getElementById("app-7"),     
    OrderedList()
)



// computed property

/*

<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})

*/


const ComputedProps = () => {

    let message = 'Hello'
    let reversedMessage = message.split('').reverse().join('')

    return {
        view: v => [
            m("p", `Original message: "${message}"`),
            m("p", `Computed reversed message: "${reversedMessage}"`)
        ]
    }
}


m.mount(
    document.getElementById("example"),     
    ComputedProps()
)




// Class and Style Bindings

/*

<div v-bind:class="classObject"></div>
data: {
  isActive: true,
  error: null
},

computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}

*/


const BindClass = () => {
    // No magic with mithril just js

    let isActive = true
    let error = null
    let active = "active"
    let text_danger = 'text-danger' 
        
    function classObject() {
        
        if (isActive && !error) active = null
        if (error && error.type === 'fatal') text_danger = null

        if (active && text_danger) return active + " " + text_danger
        if (active) return active
        if (text_danger) return text_danger

    }

    return {
        view: v => m("div", {class: classObject()}, "BindClass")
    }
}


m.mount(
    document.getElementById("bind-class"),     
    BindClass()
)


// Binding Inline Styles

/*

<div v-bind:style="styleObject"></div>


data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}


*/


const BindInlineStyles = () => {

    let styleObject = {
        color: 'red',
        fontSize: '13px'
    }

    return {
        view: v => m("div", {style: styleObject}, "BindInlineStyles")
    }
}


m.mount(
    document.getElementById("bind-inline-styles"),     
    BindInlineStyles()
)



// Conditional Rendering

/*

<div v-if="Math.random() > 0.5">
  Now you see me
</div>

<div v-else>
  Now you don't
</div>

*/


const SingleIfElse =  {
    view: v => [
        Math.random() > 0.5 ? 
        m('div', "Now you see me") : m('div', "Now you don't") 
    ]  
}


// Use render when you want to render it yourself 
// mount listens to changes and renders it each time data changes

m.render(
    document.getElementById("SingleIfElse"),     
    m(SingleIfElse)
)


// Multiple if else rendering

/*

<div v-if="type === 'A'">
  A
</div>

<div v-else-if="type === 'B'">
  B
</div>

<div v-else-if="type === 'C'">
  C
</div>

<div v-else>
  Not A/B/C
</div>

*/


let type = "A"


const MultipleIfElse =  {
    view: v => {

        if (type ==='A') return m("div", "A")
        else if (type ==='B') return m("div", "B")
        else if (type ==='C') return m("div", "C")
        else return m("div", "Not A/B/C")

    }
}


m.render(
    document.getElementById("MultipleIfElse"),     
    m(MultipleIfElse)
)



// Components Basics

/*

// Define a new component called button-counter

Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})

<div id="components-demo">
  <button-counter></button-counter>
</div>

new Vue({ el: '#components-demo' })


*/


const ButtonCounter = () => {
    
    let count = 0

    return {
        view: v => m("button", {onclick: _ => count++}, `You clicked me ${count} times.`)
    }

}


m.mount(
    document.getElementById("components-demo"),     
    ButtonCounter()
)


// Reusing Components

/*

<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>

*/

// Same component made up > ButtonCounter 


const Buttons = {
    view: v => [
        m(ButtonCounter),
        m(ButtonCounter),
        m(ButtonCounter)
    ]
}


m.mount(
    document.getElementById("reusing-components-demo"),   
    Buttons
)



// Props

/*

Vue.component('blog-post', {
  // camelCase in JavaScript
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})


<!-- kebab-case in HTML -->
<blog-post post-title="hello!"></blog-post>

*/


const ComponentProps = {
  view: v => m("div", v.attrs.post_title)
}


m.render(
  document.getElementById("props"),   
  m(ComponentProps, {post_title:"hello!"})
)



// Routing 

/*

const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})

*/


const NotFound = { view: v => m('p', 'Page not found') }
const Home = { view: v => m('p', 'home page') }
const About = { view: v => m('p', 'about page') }

const routes = {
  '/home': Home,
  '/about': About,
  "/:404...": NotFound
}

m.route(document.getElementById("routes"), "/home", routes)
// http://127.0.0.1:5500/#!/about







// State management

/*
var sourceOfTruth = {}

var vmA = new Vue({
  data: sourceOfTruth
})

var vmB = new Vue({
  data: sourceOfTruth
})


var store = {
  debug: true,
  state: {
    message: 'Hello!'
  },
  setMessageAction (newValue) {
    if (this.debug) console.log('setMessageAction triggered with', newValue)
    this.state.message = newValue
  },
  clearMessageAction () {
    if (this.debug) console.log('clearMessageAction triggered')
    this.state.message = ''
  }
}

var vmA = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
})

var vmB = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
})

*/


var store = {
  debug: true,
  state: {
    message: 'Hello!'
  },
  setMessageAction (newValue) {
    if (this.debug) console.log('setMessageAction triggered with', newValue)
    this.state.message = newValue
  },
  clearMessageAction () {
    if (this.debug) console.log('clearMessageAction triggered')
    this.state.message = ''
  }
}


const ComponentA = {
  view: v => m("div", [
    m("p", "ComponentA state: " + JSON.stringify(store.state)),
    m("input", {id:"A"}),
    m("button", { onclick: e => {
      
      let newValue = document.getElementById("A").value
      store.setMessageAction(newValue)

    }}, "Set state")
  ])
} 


const ComponentB = {
  view: v => m("div", [
    m("p", "ComponentB state: " + JSON.stringify(store.state)),
    m("button", {onclick: e => {
      store.clearMessageAction()
    }}, "Clear state")
  ])
} 


const ComponentsAB = {
  view: v => [
    m(ComponentA),
    m(ComponentB)
  ]
}


m.mount(
  document.getElementById("state"),
  ComponentsAB
)



```