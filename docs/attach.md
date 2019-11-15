# lazy.attach

This function allows you to instantiate new components from a template and attach the component to an existing html element.

```html
<body>
    <div id="myElement" class="container"></div>
</body>
```

### From URL

```html
<script>
    lazy.attach({ url: "contact.html", element: myElement })
</script>
```

Your component's template is declared in a separate file. The .html extension is irrelevant. This could be  .aspx or .php, etc.

Also, you can use CSS query selectors instead of an actual element:

```html
<script>
    lazy.attach({ url: "contact.html", element: ".container" })
</script>
```

### From Cache

```html
<script>
    lazy.attach({ name: "contact", element: myElement })
</script>
```
This means that the component has already been compiled and cached. This happens when you have previously loaded the component or when you manually compile the component using `lazy.compile`. If the component is not cached, then the function will try to load it from a script reference (if any).

```html
<script type="lazy/contact" src="contact.html"></script>

<script>
    lazy.attach({ name: "contact", element: myElement })
</script>
```

The script reference must contain a type of "lazy/[your-tag-name]" as shown above.

### From HTML

```html
<script>
    lazy.attach({ html: myHtml, element: myElement })
</script>
```

You have your component's template html stored somewhere and you want to use it.

### Promise

The `lazy.attach` function returns a promise that resolves when the component has been created.

```html
<script>
    lazy.attach({ html: myHtml, element: myElement })
    .then(function(tag){
        document.body.appendChild(tag.root)
    }).catch(function(){
        alert("Oh, oh, something happened.")
    })
</script>
```

`root` is your element that can be added to the DOM. `tag` has all the properties and functions that you declared in your component's template.

### Properties

You can pass data to your components when instantiating them.

```html
<script>
    lazy.attach({ url: "contact.html", element: myElement, props: { name: "John" } })
</script>
```
And in your component, you would consume it like this:

```html
<contact>
    <div>{props.name}</div>
</contact>
```
