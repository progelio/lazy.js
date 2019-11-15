# lazy.mount

This function allows you to instantiate new components mounted on empty tags on your html document.

```html
<body>
    <contact></contact>
    <div is="contact"></div> /*same as above*/
</body>
```

Then, you would mount your component by using any of the below options.

### From URL

```html
<script>
    lazy.mount({ url: "contact.html" })
</script>
```

Your component's template is declared in a separate file. The .html extension is irrelevant. This could be  .aspx or .php, etc.

### From Cache

```html
<script>
    lazy.mount({ name: "contact" })
</script>
```
This means that the component has already been compiled and cached. This happens when you have previously loaded the component or when you manually compile the component using `lazy.compile`. If the component is not cached, then the function will try to load it from a script reference (if any).

```html
<script type="lazy/contact" src="contact.html"></script>

<script>
    lazy.mount({ name: "contact" })
</script>
```

The script reference must contain a type of "lazy/[your-tag-name]" as shown above.

### From HTML

```html
<script>
    lazy.mount({ html: myHtml })
</script>
```

You have your component's template html stored somewhere and you want to use it.

### Promise

The `lazy.mount` function returns a promise that resolves when the component has been created.

```html
<script>
    lazy.mount({ html: myHtml })
    .then(function(tags){
        console.log(tags) //logs a list of mounted tags
    }).catch(function(){
        alert("Oh, oh, something happened.")
    })
</script>
```

### Properties

You can pass data to your component during mount.

```html
<script>
    lazy.mount({ url: "contact.html", props: { name: "John" } })
</script>
```
And in your component, you would consume it like this:

```html
<contact>
    <div>{props.name}</div>
</contact>
```

You can also pass data to you component via attributes:

```html
<body>
    <contact name="John"></contact>

    <script>
        lazy.mount({ url: "contact.html" })
    </script>
</body>
```

The data that you pass around can be dynamic too:

```html
<body>
    <contact name="{contactName}"></contact>

    <script>
        lazy.mount({ url: "contact.html" })
    </script>
</body>
