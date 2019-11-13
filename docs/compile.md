# lazy.compile

This function will compile your html and return a promise that resolves to a template. This is used to cache your components for later use. It also serves to make dynamic components.

```html
<script>
    lazy.compile("contact.html")
    .then(function(template){
        console.log(template)
    }).catch(function(x){
        alert("Oops, something happened.")
    })
</script>
```

