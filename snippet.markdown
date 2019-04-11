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

2) Describe the merits of statically typed languages, as opposed to ('5 * "16 kittens" + "10" == 90').

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

4) Explain the difference between Encapsulation and Abstraction to a 12 years old child.

5) What is the dot product of two vectors and can you list a few examples where it can be useful in computer graphics?

6) What are the typical matrices you may use in a vertex shader?

7) The WebGL application you are developing seems to run slow on mobile and consumes too much memory, makes the browser crash. What ideas would you try to help the situtation?