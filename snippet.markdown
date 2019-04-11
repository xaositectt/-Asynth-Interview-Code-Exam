1) Fix the possible bug below. Explain the problem.

```html
<html>
	<head>
		<script src="jquery-3.2.1.min.js"></script>
	</head>
	<body>
		<a id="myLink" href="#">Test</a>
		<script>
			$("#myLink").on("click", function() {
				$.ajax({
					success: function() {
						console.log(this.id);
					}
				});
			});
		</script>
	</body>
</html>
```

**My answer:**

notes:

-  There is no doctype declaration
- I downloaded the jquery file
- Chrome doesn't allow ajax methods because of CORS policy, we can open it in Mozilla

the bug:

- When the function is triggered, the "this" reference is evaluated. In this case the "this" refers to the object in the $ajax() parentheses, and that object doesn't have an "id" property  so it logs undefined.
- If I put the id property in the object it logs it, now it will log "something":


```html
	<!DOCTYPE html>
	<html>
		<head>
			<script src="jquery-3.2.1.min.js"></script>
		</head>
		<body>
			<a id="myLink" href="#">Test</a>
			<script>
				$("#myLink").on("click", function() {
					$.ajax({
                        id: 'something',
						success: function() {
							console.log(this.id);
						}
					});
				});
			</script>
		</body>
  </html>
```

- If I want it to access the anchor tag's id, I have to give it access to the "this" of the event handler function, I can do it a couple of different ways. Way number one: store it in a variable and send reference it in the success function. This solution avoids the problem of understanding the "this" binding (will only show the script tag part of code from now)

```html
<script>
	$("#myLink").on("click", function() {
		var myLinkThis = this;
		$.ajax({
			success: function() {
				console.log(myLinkThis.id);
			}
		});
	});
</script>
```

- Use an ES6 arrow function that inherits the "this" value from its closest outer scope

```html
<script>
    $("#myLink").on("click", function() {
        $.ajax({
            success: () => {
              console.log(this.id);
            }
        });
    });
</script>
```

- We can bind the "this" of the outer scope to the function with the built in bind

```html
<script>
    $("#myLink").on("click", function() {
        $.ajax({
            success: function() {
              console.log(this.id);
            }.bind(this)
        });
    });
</script>
```

- We can bind it to the function in other ways too like:

```html
<script>
    $("#myLink").on("click", function() {
        $.ajax({
            success: function() {
              console.log(this.id);
            }.call(this)
        });
    });
</script>
```

```html
<script>
    $("#myLink").on("click", function() {
        $.ajax({
            success: function() {
              console.log(this.id);
            }.apply(this)
        });
    });
</script>
```


2) Describe the merits of statically typed languages, as opposed to ('5 * "16 kittens" + "10" == 90').

**My answer:**

3) Fix the bug below.

```javascript
function contains(haystack, needle)
{
	for (const element of list) {
		if (element == needle) return true;
		return false;
	}
}
```

**My answer:**

4) Explain the difference between Encapsulation and Abstraction to a 12 years old child.

5) What is the dot product of two vectors and can you list a few examples where it can be useful in computer graphics?

6) What are the typical matrices you may use in a vertex shader?

7) The WebGL application you are developing seems to run slow on mobile and consumes too much memory, makes the browser crash. What ideas would you try to help the situtation?