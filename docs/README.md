# lazy.js

A simple javascript library to create simple UI components that can be lazy-loaded via Promises.

* [Templates](https://github.com/progelio/lazy.js/blob/master/docs/templates.md)
* [lazy.create](https://github.com/progelio/lazy.js/blob/master/docs/create.md)
* [lazy.mount](https://github.com/progelio/lazy.js/blob/master/docs/mount.md)
* [lazy.attach]()
* [lazy.compile]()
* [lazy.http]()
* [lazy.route]()


## How to use it.

Include the script file in your html document:

```html
<script src="lazy.min.js"></script>
```

Create a separate html file in which you will declare your component template:

```html
<todo>
    <h1>My Todo List</h1>

    <div>
        <input ref="itemName" type="text" />
        <button onclick="{ add }">Add</button>
    </div>

    <ul>
        <li each="{x in items}">
            <span>{x.name}</span>
            <button onclick="{ remove }">Remove</button>
        </li>
    </ul>

    <style>
        :root { display:block }
        h1 { color:blue }
        li { margin-bottom: 10px }
    </style>

    <script>
        this.items = [{ name: "Milk" }, { name: "Eggs" }, { name: "Juice" }]

        this.add = function (e) {
            this.items.push({ name: this.refs.itemName.value })
            this.update()
            this.refs.itemName.value = ""
        }

        this.remove = function (e) {
            this.items.splice(e.item.index,1)
            this.update()
        }
    </script>
</todo>
```

To consume your component:

```html
<body>
    <script>
        lazy.create({
            url: "todo.html"
        }).then(function (tag) {
            document.body.appendChild(tag.root)
        })
    </script>
</body>
```

### Mount Using Declared Tags

In your html document, where you want to consume your custom component, declare your html tag like below:

```html
<body>
    <todo></todo>

    <script>
        lazy.mount({
            url: "todo.html"
        })
    </script>
</body>
```

Your todo tag will be replaced with an instance built from your custom component template.

### Mount Using Script Reference

```html
<body>
    <todo></todo>

    <script type="lazy/todo" src="todo.html"></script>
    <script>
        lazy.mount({
            name: "todo"
        })
    </script>
</body>
```

If you have multiple components, you can use the wild card "*" as shown below:

```html
<body>
    <todo></todo>
    <contact></contact>

    <script type="lazy/todo" src="todo.html"></script>
    <script type="lazy/contact" src="contact.html"></script>
    
    <script>
        lazy.mount({
            name: "*"
        })
    </script>
</body>
```

### Mount Using HTML

```html
<body>
    <hello-world></hello-world>

    <template id="myHtml">
        <hello-world>
            <h2>Hello World!</h2>
            <p>The time is {time}.</p>

            <style>
                :root { color: blue; display: block }
            </style>

            <script>
                this.time = new Date().toLocaleTimeString()
            </script>
        </hello-world>
    </template>

    <script>
        lazy.mount({
            html: myHtml.innerHTML
        })
    </script>
</body>
```

### Attach Components to an Existing Element

```html
<body>
    <div class="container"></div>

    <script>
        lazy.attach({
            url: "todo.html",
            element: ".container"
        })
    </script>
</body>
```

The "element" property may take a css querySelector string or an actual element. 
The `lazy.attach` funtion also takes "name" or "html" properties just like `lazy.mount`. 
