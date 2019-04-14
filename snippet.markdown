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

In statically or strongly typed languages the type is fixed, either explicitly given by the developer or "guessed" by the compiler and the correct types are checked before run time. The advantage is that bugs related to type checking can be avoided this way.

Also if a variable is given a type it cannot be changed later.

Example: c++, compiler throws a type error even though the code in the if branch never gets executed

```c++
#include <iostream>
#include <string>

int main()
{
  std::string name = "Roberta";
  if (false) {
    std::cout << name + 3 << "\n";
  }
}

```

Strongly typed languages don't allow confusing behaviors like weird implicit type coercions that can have many rules (in Javascript) and are sometimes hard to understand.

Javascript is a dynamically typed language so the type checking is at run time. This means that there can be bugs left hidden in the code because only the parts of the code that actually get executed get type checked. In my example the toUpperCase() function expects a string so it should throw an error but it gets ignored at runtime.

```javascript
let num = 3;
// if it's outside of the if scope, it throws an error
// console.log(num.toUpperCase())
if (false) {
    // it doesn't throw an error this way
    console.log(num.toUpperCase())
}
```

The dynamically typed Javascript also allows behaviors like the explicit change of types or implicit type coercion, where the a type is converted into another type, which can be confusing and hard to understand. Examples:

```javascript
false == 0 // logs true 
[] + 3 // turns into "3"
5 == "5" // logs true
```

In some cases I can force stricter rules for example these will log false

```javascript
false === 0 // logs false 
5 === "5" // logs false
```

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

notes:

- in the original function the argument list had "haystack" and the for ... in had "list", so I fixed that.
- I changed the == to === to make the equality check stricter

The intended bug was due to the function always returning after the first iteration of the for ... in statement. A function quits executing after it returns a value, and we need to iterate through the whole haystack to check all elements, not just the first one, and we can achieve that by storing the boolean value in a variable.

```javascript
function containsOriginal(haystack, needle) {
  for (const element of haystack) {
    if (element == needle) return true;
    return false;
  }
}

// fixed version
function contains(haystack, needle) {
  let result = false;
  for (const element of haystack) {
    if (element === needle) {
      result = true;
    }
  }
  return result;
}

const haystack = [1, 2, 3];

console.log(contains(haystack, 2)); // logs true
console.log(containsOriginal(haystack, 2)); // logs false
```



4) Explain the difference between Encapsulation and Abstraction to a 12 years old child.

5) What is the dot product of two vectors and can you list a few examples where it can be useful in computer graphics?

6) What are the typical matrices you may use in a vertex shader?

7) The WebGL application you are developing seems to run slow on mobile and consumes too much memory, makes the browser crash. What ideas would you try to help the situtation?