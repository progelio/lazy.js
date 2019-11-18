# Templates

A component is a simple html tag with an inner body. It contains three parts: `html`, `style` and `script`. Example:

```html
<contact>
    <div>Name: {name}</div>
    <div>Age: {age}</div>
    <div class="gender">Gender: {gender}</div>
    <div>Amount: {formatCurrency(100)}</div>    

    <div>Time: {new Date()}</div>

    <style>
        :root { display:block }
        div { color:blue }
        .gender { font-weight:bold }
    </style>

    <script>
        this.name = "John"
        this.age = 45
        this.gender = "Male"

        this.formatCurreny = function(amount){
            return "$" + amount + ".00"
        }
    </script>
</contact>
```

`style` and `script` are optional.

Your component can be declared as a standalone element in an html file or anywhere in your html document - like inside of a template tag.

## HTML

The html may contain merge-fields like: `{fieldName}`. These are dynamic placeholders for the value of your fields.

Anything you type between the curly brackets `{...}` will be evaluated. You can do `{ new Date() }` or call a function `{ formatCurrency(12) }`.
You can also access window level variables by doing `{ window.myVariable }`.

Merge-fields are run in the scope of your script tag as shown in the above example.

#### The `ref` Attribute

If you need to reference an element within your component, you can use the `ref` attribute. Example:

```html
<contact>
    <div ref="name"><div>

    <script>
        this.onCreate = function(){
            this.refs.name.innerHTML = "John"
        }
    </script>
</contact>
```

If multiple elements with the same `ref` name are found, the last one will be used.

#### The `if` Directive

Used to make an element render conditionally:

```html
<contact>
    <div>{name}<div>

    Interested in:
    <div if="{showInterests}">Girls</div>

    <script>
        this.name = "John"
        this.showInterests = false
    </script>
</contact>
```

#### The `each` Directive

Used to repeat an element:

```html
<contact>
    <div>{name}<div>

    Interested in:
    <div if="{showInterests}">
        <ul>
            <li each="{x in items}">{index} - {x}</li>
        </ul>
    </div>

    <script>
        this.name = "John"
        this.showInterests = true
        this.items = ["Cars", "Computers", "Boats"]
    </script>
</contact>
```

The `index` property is automatically declared and only lives in the scope of the element that is repeated. You can have nested repeaters as well and you can also declare your own `index` variable name. Example:

```html
<contact>
    <div>{name}<div>

    <ul>
        <li each="{country,countryIndex in countries}">
            {countryIndex} - {country.name}

            <div each="{state,stateIndex in country.states}" style="padding-left:20px">
                {stateIndex} - {state}
            </div>
        </li>
    </ul>

    <script>
        this.name = "John"
        this.countries = [
            {
                name: "United States",
                states: ["FL", "NY", "CA"]
            },
            {
                name: "Canada",
                states: ["NU", "ON", "PE"]
            }
        ]
    </script>
</contact>
```

#### The `show` Directive

Used to show or hide an element via the CSS property `display:none`:

```html
<contact>
    <div>{name}<div>

    Interested in:
    <div show="{showAge}">
        45
    </div>

    <script>
        this.name = "John"
        this.showAge = false
    </script>
</contact>
```

If the value is falsy, then the element will be `display:none`.

#### The `visible` Directive

Used to show or hide an element and reserve its visual allocated space using the CSS property `visibility:hidden`:

```html
<contact>
    <div>{name}<div>

    Interested in:
    <div visible="{showAge}">
        45
    </div>

    <script>
        this.name = "John"
        this.showAge = false
    </script>
</contact>
```

#### Ternary `true ? true : false` Shortcut `value:condition`

You can use a ternary shortcut anywhere on your template. Example:

```html
<div class="{is-loading:true}"></div>
```

becomes:

```html
<div class="is-loading"></div>
```

The condition can be any truthy or falsy value.

## Style

To style any instance of your component, include a style tag direcly under your main component declaration. Example:

```html
<contact>
    <div>Name {name}</div>

    <style>
        :root { display:block }
        div { color:blue }
    </style>

    <script>
        this.name = "John"
    </script>
</contact>
```

`:root` represents your component's tag name. In the above case, it becomes `contact`. So, 

```html
<style>
    :root { display:block }
    div { color:blue }
</style>
```

Becomes,

```html
<style>
    contact { display:block }
    contact div { color:blue }
</style>
```

The final style code is dynamically injected in the `head` portion of your html document and removed from your final rendered component. Any query selector declared in the style tag will only apply to elements within the component.

## Script

The script portion of your component is removed from the final rendered version of the compoenent. It may contain properties and functions.

There are a few reserved keywords that should only be used in the context of which they are intended:

`refs`, `ready`, `destroy`, `update`, `onInit`, `onCreate`, `onCreated`, `onUpdate`, `onUpdated`, `onDestroyed`, `props`

```html
<contact>
    <div>
        Name:
        <span ref="name">{name}</span>
    </div>

    <button onclick="{ changeName }">Change Name</button>
    <button onclick="{ getName }">What is my name?</button>
    <button onclick="{ remove }">Destroy</button>

    <style>
        :root { display: block }
        div { color: blue }
    </style>

    <script>
        this.name = "John"

        this.changeName = function(e) {
            this.name = "Peter"
            this.update() //update the UI to reflect the change.
        }

        this.getName = function (e) {
            alert(this.refs.name.innerHTML)
        }

        this.remove = function (e) {
            this.destroy(false)
        }

        this.onInit = function () {
            //after initialized
            this.ready()
        }

        this.onCreate = function () {
            //before create
        }

        this.onUpdate = function () {
            //before update
        }

        this.onUpdated = function () {
            //after update
        }

        this.onCreated = function () {
            //after create
        }

        this.onDestroyed = function () {
            //after destroyed
        }
    </script>
</contact>
```

All life-cycle event names start with the word `on` like `onCreated`, etc. The order of the events in which they are executed is:

`onInit`, `onCreate`, `onUpdate`, `onUpdated`, `onCreated`

`onDestroyed` is only called when the tag has been removed from the DOM via the `tag.destroy` function.

The `destroy` function takes a truthy value to keep or remove the parent tag. Example:

```
this.destroy(true)
```

This will keep the parent tag in the DOM like `<contact></contact>`. The inner part is removed. The default value is `false` if you don't pass anything.

## onInit

The `onInit` event allows you to do things before the component is loaded into the DOM. For example, you need to fetch some data from the server, etc.

After your task is complete, you must call the `this.ready()` function so the component is loaded. The `this.ready()` function is always required if the `onInit` event is used.

## props

This is a property that is used to pass data to your component.

```html
<contact>
    <div>{props.name}<div>

    <script>
        this.onCreate = function () {
            console.log(props.name) //available here too.
        }
    </script>
</contact>
```
You can pass the value to your component when you instantiate the component:

```html
<script>
    lazy.create({ url: "contact.html", props: { name: "John" } })
</script>
```

Or, if you mount your component:

```html
<body>
    <contact name="John"></contact>

    <script>
        lazy.mount({ url: "contact.html" })
    </script>
</body>
```

#### Child Components

You can create components that have other child components in them. Take a look at the `todo` component below:

```html
<todo>
    <contact></contact>

    <div>My {title} Todo List</div>
    ...
    <script type="lazy/contact" src="contact.html"></script>

    <style>
        :root { display:block }
    </style>

    <script>
        this.title = "Shopping"
    </script>
</todo>
```

The child component is automatically mounted because you have a script reference there. There is no need to use `lazy.mount`.

#### The `is` Attribute

You can mount a component on a standard tag by using the `is` attribute.

```html
<todo>
    <div is="contact"></div>
    <script type="lazy/contact" src="contact.html"></script>
</todo>
```
