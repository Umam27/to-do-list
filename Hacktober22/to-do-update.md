Google's Game of the Year features a playful CSS animation on the homepage, with the title words tumbling and bumping into one another. Here's how it was done. 

The first step is to define the webpage document with HTML. It consists of the HTML document container, which stores a head and body section. While the head section is used to load the external CSS and JavaScript resources, the body is used to store the page content.
```
<!DOCTYPE html>
<html>
<head>
<title>Off Kilter Text Animation</title>
<link rel="stylesheet" type="text/css" href="styles.css"/>
<script src="code.js"></script>
</head>
<body>
  <h1 class="animate backwards">The Animated Title</h1>
  <h1 class="animate forwards">The Animated Title</h1>
  <h1 class="animate mixed">The Animated Title </h1>
</body>
</html>
```
The page content consists of three h1 title tags that will show the different variations of the animation effect. While any text can be inserted into these tags, their animation is defined by the names in the class attribute. The presentation and animation settings for these class names will be defined in the CSS later on.

Next, create a new file called 'code.js'. We want to find all page elements with the animate class and create an array list representing each word of the inner text. The initial animation delay is also defined in this step. Page content is not available until the page has fully loaded, so this code is being placed inside the window’s load event listener.

The word content of the animation items needs to be contained inside a span element. To do this, the existing HTML content is reset to blank, then a loop is used to make the word in the identified 'words' list a span element. Additionally, an animationDelay style is applied – calculated in relation to the initial delay (specified below) and the word’s index position.
```
window.addEventListener("load", function(){
	var delay = 2;
	var nodes = document.querySelectorAll
(".animate");
	for(var i=0; i<nodes.length; i++){
		var words = nodes[i].innerText.split(" ");
		nodes[i].innerHTML = "";
for(var i2=0; i2<words.length; i2++){
			var item = document.createElement("span");
			item.innerText = words[i2];
			var calc = (delay+((nodes.length + i2)/3));
	item.style.animationDelay = calc+"s";
			nodes[i].appendChild(item);
}
	}
});```

Create a new file called styles.css. Now we'll set the presentation rules that will be part of every word element in the animation, controlled by their span tag. Display as block, combined with centred text alignment, will result in each word appearing on a separate line horizontally aligned to the middle of its container. Relative positioning will be used to animate in relation to its text-flow position.
```
.animate span{
	display: block;
	position: relative;
	text-align: center;
}
```
Animation elements that have the backwards and forwards class have a specific animation applied to them. This step defines the animation to apply to span elements whose parent container has both the animate and backwards or forwards class. 

Note how there is no space between the animate and backwards class reference, meaning the parent element must have both.
```
.animate.backwards > span{
	animation: animateBackwards 1s ease-in-out 
forwards;
}
.animate.forwards > span{
	animation: animateForwards 1s ease-in-out 
forwards;
}
```
The mixed animation is defined using the same settings used for the forwards and backwards animations. Instead of applying the animations to every child of the parent, the nth-child selector is used to apply alternating animation settings. The backwards animation is applied to every even-number child, while the forwards animation is applied to every odd-number child.
```
.animate.mixed > span:nth-child(even){
	animation: animateBackwards 1s ease-in-out 
forwards;
}
.animate.mixed > span:nth-child(odd){
	animation: animateForwards 1s ease-in-out 
forwards;
}
```
The animations we've just created are made with an initial 'from' starting position, with no vertical position or rotation adjustment. The 'to' position is the final state of the animation, which sets the elements with an adjusted vertical position and rotation state. Slightly different ending settings are used for both animations to avoid the text becoming unreadable due to overlap in mixed animations.
```
@keyframes animateForwards {
	from { top: 0; transform: rotate(0deg); }
	to { top: .9em; transform: rotate(-15deg); }
}
@keyframes animateBackwards {
	from { top: 0; transform: rotate(0deg); }
	to { top: 1em; transform: rotate(25deg); }
}
```
