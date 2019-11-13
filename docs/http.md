# lazy.http

Make a web request and returns a promise.

```html
<script>
    lazy.http({ url: "/users.aspx" })
    .then(function (result) {
        console.log(result.status)
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```

You can also do `POST` or anything else.

#### Form Post

```html
<script>
    lazy.http({ 
        url: "/users.php", 
        method: "POST", 
        type: "form", //becomes: application/x-www-form-urlencoded
        data: "name=John" 
    }).then(function (result) {
        console.log(result.status)
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```

#### JSON Post

This option allows you to pass an object which is serialized before it is sent.

```html
<script>
    lazy.http({ 
        url: "/api/users", 
        method: "POST", 
        type: "json", //becomes: application/json
        data: { name: "John" }
    }).then(function (result) {
        console.log(result.status)
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```

#### Headers

```html
<script>
    lazy.http({ 
        url: "/api/users", 
        method: "POST", 
        type: "json", //becomes: application/json
        headers: { "Authorization": "abcdefg1235" }
        data: { name: "John" }
    }).then(function (result) {
        console.log(result.status)
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```

### Shortcuts

If you like to consume JSON APIs, the below shortcuts could be used. 

#### lazy.http.get

```html
<script>
    lazy.http.get("/api/users/123")
    }).then(function (result) {
        console.log(result.status)
        console.log(result.data) //deserialized object
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```

#### lazy.http.post

```html
<script>
    lazy.http.post("/api/users", { name: "John" })
    }).then(function (result) {
        console.log(result.status)
        console.log(result.data) //deserialized object
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```

#### lazy.http.delete

```html
<script>
    lazy.http.delete("/api/users", { name: "John" })
    }).then(function (result) {
        console.log(result.status)
        console.log(result.data) //deserialized object
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```
#### lazy.http.put

```html
<script>
    lazy.http.put("/api/users", { name: "John" })
    }).then(function (result) {
        console.log(result.status)
        console.log(result.data) //deserialized object
        console.log(result.text)
        console.log(result.xhr)
    }).catch(function (x) {
        alert("Oops, something happened.")
    })
</script>
```

You can pass headers in two ways. You can set them globally like:

```html
<script>
    lazy.http.headers["Content-Type"] = "application/json"
    lazy.http.headers["Authorization"] = "abcde12345"
    lazy.http.get("/api/users/123")
</script>
```

Or, you can set them on a per call basis:

```html
<script>
    lazy.http.get("/api/users/123", { "Authorization": "abcde12345" })
    lazy.http.post("/api/users/123", { name: "John" }, { "Authorization": "abcde12345" })
</script>
```
