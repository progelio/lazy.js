# lazy.create

Once you have a template created, you can instantiate new components from it in many ways.

### From URL

```html
<script>
    lazy.create({ url: "contact.html" })
</script>
```

Your component's template is declared in a separate file. The .html extension is irrelevant. This could be  .aspx or .php, etc.

### From Cache

```html
<script>
    lazy.create({ name: "contact" })
</script>
```
This means that the component has already been compiled and cached. This happens when you have previously loaded the component or when you manually compile the component using `lazy.compile`. If the component is not cached, then the function will try to load it from a script reference (if any).

```html
<script type="lazy/contact" src="contact.html"></script>

<script>
    lazy.create({ name: "contact" })
</script>
```

The script reference must contain a type of "lazy/[your-tag-name]" as shown above.

### From HTML

```html
<script>
    lazy.create({ html: myHtml })
</script>
```

You have your component's template html stored somewhere and you want to use it.

### Promise

The `lazy.create` function returns a promise that resolves when the component has been created.

```html
<script>
    lazy.create({ html: myHtml })
    .then(function(tag){
        document.body.appendChild(tag.root)
    }).catch(function(){
        alert("Oh, oh, something happened.")
    })
</script>
```

`root` is your element that can be added to the DOM. `tag` has all the properties and functions that you declared in your component's template.

### Options

You can pass objects to your components when instantiating them.

```html
<script>
    lazy.create({ url: "contact.html", opts: { name: "Contact" } })
</script>
```
And in your component, you would consume it like this:

```html
<contact>
    <div>{opts.name}</div>
</contact>
```
