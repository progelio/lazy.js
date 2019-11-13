# lazy.router

This will allow you to create single-page applications. This is used to route your pages using your hash value from the URL.

Let's pretend that the URL is `http://sample.com` and you want to execute a function when the URL matches `http://sample.com/#/books/arthur/123`.

First, you need to add your routes using the `lazy.router.add` function.

```html
<script>
    lazy.router.add("books/arthur/123", function () {
        //do something
        console.log(this.oldUrl)
        console.log(this.newUrl)
    })

    lazy.router.start(true)
</script>
```

`lazy.router.start()` will start the router to listen for hash changes. 

If you do `lazy.router.start(true)`, then the router will start listening and the current hash value will be executed.

#### Dynamic Values

```html
<script>
    lazy.router.add("books/arthur/[0-9]+", function () {
        //do something
        console.log(this.oldUrl)
        console.log(this.newUrl)
    })

    lazy.router.start(true)
</script>
```

As you can see, you can use regex as part of your match. If you want to capture the value so you can do further processing, all you have to do is wrap the value in parentheses.

```html
<script>
    lazy.router.add("books/arthur/([0-9]+)", function (id) {
        if (id == 0) {
            return false
        } else {
            //do something
            console.log(this.oldUrl)
            console.log(this.newUrl)
        }
    })

    lazy.router.start(true)
</script>
```

In the above scenario, if the id is zero, the route is canceled by returning false.

#### Multiple Captures

```html
<script>
    lazy.router.add("books/([a-z]+)/([0-9]+)", function (author, id) {
        if (id == 0) {
            return false
        } else {
            //do something
            console.log(this.oldUrl)
            console.log(this.newUrl)
        }
    })

    lazy.router.start(true)
</script>
```

Routes are matched in the order in which the are added. This means, if you want to handle 404, in the case where no matches where found, you last match would be:

```html
<script>
    lazy.router.add("books/([a-z]+)/([0-9]+)", function (author, id) {
        if (id == 0) {
            return false //match will be skipped.
        } else {
            //do something
            console.log(this.oldUrl)
            console.log(this.newUrl)
        }
    })

    lazy.router.add(".*", function () {
        //do something and/or load a 404 page.
        console.log(this.oldUrl)
        console.log(this.newUrl)
    })

    lazy.router.start(true)
</script>
```

Requesting `http://sample.com/#/books/arthur/0` will trigger your 404 page route because the id is zero and it would be skipped in the first route rule.

#### lazy.router.base

You can set the base of your routes to be whatever you want. The default value is `#/`

```html
<script>
    lazy.router.base("#!")
</script>
```

