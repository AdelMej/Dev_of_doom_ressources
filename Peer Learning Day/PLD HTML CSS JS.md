# Advanced HTML
What is the mandatory first line in an HTML5 document, and what is its purpose?
<`DOCTYPE html`>
it's purpose is to define that the doc is a html file because other languages like xml can use .html files

What is the difference between the <`html`>, <`head`>, and <`body`> tags in a web page structure?
html is the entire structure containing the body and other tags
head contains all the metadatas including the title the encoding and the information for mobile etc...
body is where the core of the code is and will be it's where the content of the  page will be coded

What is the role of the <`meta`> tag, and what do the charset and viewport attributes do?
meta tag is to add metadatas it's datas that are hidden from the user but can be used by the browser

Why is it important to define the lang attribute in the <`html`> tag?
it's important to definite the lang attribvute because different lang can have different structures and aren't read the same way

What is the difference between a semantic and a non-semantic HTML tag?
a semantic tag has a name a none semantic one is generic example <`img`> <`section`> <`article`>
non semantic example <`div`> and <`span`>

Give three examples of semantic HTML tags and briefly describe their roles.
<`img`> used for displaying pictures
<`section`> used to define a section
<`article`> used to define an article generaly inside a session
<`ul`> unordered list (un point)

What is the semantic meaning of each of the following tags: <`header`>, <`main`>, <`footer`>, <`article`>, <`nav`>, <`section`>, <`aside`>?
header represent the header top side of a section
main is where the ain content will be located it's generaly betweenn header and footer
article goes inside a section and is there to define articles which are generaly text content combined with pictures
nav is to define the navbar can be mobile or not depending on architecture
section defines a structural section used to structure the html to have umltiple sections with different content
aside is used for representing content that isn't directly relevant to the main content

What is the difference between <`div`> and <`span`>? Provide a concrete example for each.
divb is used for a generic box in the code
span is used for text formating

Why is heading hierarchy (from <`h1`> to <`h6`>) important, and what are the common mistakes to avoid?
h1 bigger than H6 and the size depend on the heading common mistakes to avoid is to not nest tags in headings

What is considered a good practice for organizing the main content of a web page?
having a header a lmain and a footer with relevant content

How do you create an ordered list and an unordered list? Which tags are used?
ul unorder list ol for ordered list
<`ul`> <`ol`> <`il`>

What are the main differences between SVG, GIF, PNG, JPG and WEBP image formats?
image file size
animation


How do you properly include an image in a webpage, and which attributes are essential for accessibility?
<`img src="" alt=""`>
<`track`>

Which HTML tag should be used for a short quotation? And which one for a long quotation?
<`blockquote`> long citation
<`q`> for small citation

How do you structure a table in HTML? What is the difference between <`th`>, <`tr`>, and <`td`>?
<`table`>
	<`thead'>
		<`tr`>
			<`th`>
			<`td`>
		<`/tr`>
	<`tbody`>
		<`tr`>
			<`th`>
			<`td`>
		<`/tr`>
	<`tfoot`>
		<`tr`>
			<`th`>
			<`td`>
		<`/tr`>

What is the purpose of the scope attribute in a table header cell (<`th`>)?
scope defines what the data scop is either a colomn or a row

Which tag is used to embed a video in a webpage, and what attributes are required to control playback?
<`video` control loop src="video.mp4" poster="someimg.png">

Which tag is used to embed an audio file, and what options are available to manage playback?
<`audio` loop src="someaudio.mp3">

Which tag allows you to embed external content (such as a YouTube video or a Google Map)? What precautions should be taken when using it?
<`iframe`>
looking if the embedding is secure

What are the main best practices to ensure an HTML5 document is valid, accessible, and well structured?
w3c validator
# Advanced CSS

What are selectors, properties, and values in CSS? Provide a short example explaining each.
nav {
color: `#fffff`;
}
nav is the selector that reprsents a tag in the html you can also use a class or an id
color is the property it defines the text color
`#ffffff` is the value it represent the color white

What is the difference between block-level and inline elements in CSS? Give an example of each.

nav would be a block level elment
nav ul would be an inline element of ul inside nav

What happens when you set display: inline-block; on an element?
the layout change in inline block which is block going left to right

Why do developers use a CSS reset or normalize.css, and what problem does it solve?
it's to normalize displaying from browsers to browsers

What are the three main ways to include CSS in a webpage, and what are the advantages of using external stylesheets?
Inline - by using the style attribute inside HTML elements
Internal - by using a <`style`> element in the <`head`> section
External - by using a <`link`> element to link to an external CSS file


How can you define and use CSS custom properties (variables)? Give a simple code example.
:root {
	--mycustomproperty: #ffffff;
}

What is the difference between using a CSS variable and using a traditional hardcoded value?
the difference is reusability you can reuse the same variables and then you can modify the property in root immediately

How do you organize a grid layout using floats? What are the limitations of this approach compared to CSS Grid or Flexbox?
you don't yopu need a dummy ::after block to guarantee that the layout is consistent

 How does the clearfix technique work, and why is it needed when using floats?
it works by adding an empty box after the floating element it is needed because float isn't considered as a block and therefore you end up with the structure ignoring it completely

What is the difference between webfont icons (like Font Awesome) and SVG icons? Which one is generally better for scalability and performance?
svg is generaly better for scalability and performance because it doesn't rely on a file and it also comes with desirable perks it can be modified with javascript and css while webfont icons are very limited to only a few css property

What are pseudo-classes in CSS? Provide two examples and explain when they are triggered.
pseudo classes are classes that apply to an element example
a:hover it activates when you hover the element with your mouse

What are pseudo-elements in CSS? Provide two examples and explain how they differ from pseudo-classes.
a[href] is a pseudo elemnt it's a

What does the following selector target:

.nav .nav-link::before

How can you create a linear gradient background in CSS? Provide an example.
What is the difference between transitions and animations in CSS?
How do you use the transition property to animate an element’s color when hovering over it?
What is the purpose of the transform property in CSS? Give examples of 2D and 3D transformations.
Why are vendor prefixes (like -webkit-, -moz-, -o-) sometimes necessary? Are they still important today?
What are the best practices for ensuring that a website’s CSS is consistent across different browsers?
When working on a large project (like Techium), what are some good habits to keep your CSS organized, maintainable, and scalable?
 
# JavaScript - Warm up
Why is JavaScript considered an amazing programming language? Name at least two reasons.
How do you run a JavaScript script from the command line in Ubuntu using Node.js?
What is the difference between var, let, and const when declaring variables?
Why is it generally recommended to avoid using var in modern JavaScript code?
What are the primitive data types available in JavaScript? Provide one example for each.
What happens if you try to assign a new value to a constant (const) variable?
How can you check the type of a variable in JavaScript?
How do you write an if / else if / else statement in JavaScript? Provide a short example.
What is the purpose of using comments in JavaScript, and what are the two ways to write them?
What are the main differences between a while loop and a for loop? When would you use each one?
How do the break and continue statements affect the execution of a loop?
What is a function in JavaScript, and what is the correct syntax for declaring one?
What is the return value of a function that does not include a return statement?
What is the difference between a local variable and a global variable?
What is the scope of a variable declared with let inside a block { }?
List and explain the arithmetic operators available in JavaScript.
What is an object in JavaScript, and how is it different from an array?
How can you iterate over the keys and values of a JavaScript object (dictionary)?
What does require() do in Node.js, and how is it used to import functions or modules from another file?
What is the output of the following code, and why?

const x = 5;
function add(y) {
  return x + y;
}
console.log(add(3));

(Explain your reasoning on whiteboard)