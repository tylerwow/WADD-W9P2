# Week 9 Practical 2: The HTML5 Canvas and Libraries
Treat this practical as a choose-your-own-adventure depending on your interests and your goals for the assessment. Pick what is most interesting / useful to you rather than trying to do all of it. 

Contents:
- [Stage 1 - drawing with the HTML canvas element](#stage-1). 
- [Stage 2 - Snake clone](#stage-2) using the HTML canvas rather than p5.js, which you did in PDM 1.
- [Stage 3 - Library tutorials](#stage-3) for a selection of the libraries permitted in the assment. Even if you don't plan on using libraries for the assessment, you are encouraged to come back to these exercises at a later date as libraries are a fundamental part of web development.

## Stage 1
These drawing-focused canvas exercises will make you nostalgic for the first few weeks of PDM. They will also help you to get to know the drawing features of the canvas API, beyond what we covered in class.

### 1.1: Draw an alien
As always, create an HTML, CSS, and JS file and link the CSS and JS to your HTML. You will use these files for the rest of the exercises in this stage.

In your HTML, add a canvas element and give it an id. Set its width to 600px and its height to 400px. In CSS, give the canvas a background colour or a border so that you can clearly see its edges. 

In your JavaScript file, create the variables that you will need to work with the canvas:

```javascript
const canvas = document.getElementById("your-canvas-id");
const ctx = canvas.getContext("2d");
```
Write some code to draw an alien using rectangles, lines, and arcs. Refer to the lecture slides and the reference: [https://www.w3schools.com/tags/ref_canvas.asp](https://www.w3schools.com/tags/ref_canvas.asp). You will need the reference for the remaining exercises.

### 1.2: Try different fill styles
You have probably already used solid fill colours for your alien. Try adding a linear gradient to the background of your canvas to make is look like the night sky. Do this using the canvas API, not CSS.

Add a moon to your canvas which uses a radial gradient as the fill.

### 1.3: Add some text
Give the alien a speech bubble that contains a message. Try using `strokeText` (rather than `fillText`) with different `strokeStyles` to explore different text effects.

### 1.4: Add animation
Add animation to your canvas drawing. For example, you could:
- Animate the text so it looks like the alien is talking to the user
- Add some mouth movements to the alien as they talk
- Make the alien fly around the scene
- Make the alien wave

## Stage 2
The canvas is most useful for creating animations rather than static drawings. For this reason, this stage focuses on animating the canvas and working with user input while the drawings themselves are kept quite simple. 

To practice animation with the canvas, you will create a simplified version of [Snake](https://www.mathsisfun.com/games/snake.html). There is a video of the completed game in the practical materials. There are LOTS of Snake implementation tutorials available online, but they generally tell you exactly what code to use, which is not always a great way to learn. The following exercises provide guidance but require you to actively apply your knowledge from this module and your previous modules.

### 2.1: Draw and animate a basic snake
As with the previous exercises, create an HTML, CSS, and JS file and link the CSS and JS to your HTML. You will use these files for the rest of the exercises in this stage.

In your HTML, add a canvas element and give it an id. Set its width to 600px and its height to 400px. In CSS, give the canvas a background colour. 

In your JavaScript file, create the variables that you will need to work with the canvas:

```javascript
const canvas = document.getElementById("your-canvas-id");
const ctx = canvas.getContext("2d");
```

You can think of the canvas in Snake as a grid. For this implementation, we will use a cell size of 20px. The snake’s body is made up of segments (or squares), where each segment is the size of one cell in the grid, 20px by 20px. At the start of the game, your snake should have one segment, its head, with coordinates 0, 0.

Draw the snake on the canvas in its starting position and check that it appears as a small square in the top left corner of the canvas. Here is a template:
```javascript
ctx.beginPath(); // use this method whenever you set colours
ctx.fillStyle = "your choice of colour";
ctx.strokeStyle = "your choice of colour";
ctx.rect(x, y, 20, 20); // x, y - coordinates of the snake segment
ctx.fill();
ctx.stroke();
```

You will notice that the template draws the snake at x, y, rather than 0, 0. This is because the location of the snake will need to change every frame of the animation, so you will need to use variables to store the location over time. 

In lecture, we discussed a couple of ways to create animation: `window.requestAnimationFrame(callback)` and `setInterval(callback, interval)`. As the snake moves at regular intervals, `setInterval()` is the best choice in this case.

Animate your snake so that it moves 20px to the right at a fixed interval e.g. 300ms. Here is a template:
```javascript
setInterval(function () {
  ctx.clearRect(0, 0, canvas.width, canvas.height); // clear the canvas
  // Your code to draw the snake
  // Your code to update the snake’s position
}, 300);
```

After a while, the snake disappears off the right edge of the screen. Update your code so that when the snake goes off the right edge, it appears back on the left side of the canvas and continues to move across. This is different from other versions of Snake where it’s game over if the snake hits a wall.

## 2.2: Respond to keyboard input
The user should be able to control the snake using the arrow keys on their keyboard. You will need an event listener to capture keyboard input. You could add the event listener to the canvas, but that means the user will have to focus the canvas (e.g. click on it) in order for their input to be received. A more user-friendly approach is to add the event listener to the document itself e.g. `document.addEventListener()`.

What event should you listen for? There are two options: "keydown" and "keyup". 

The next step is to figure out how to tell which key has been pressed. Try printing the event to the console and seeing what happens when you press arrow keys e.g. 

```javascript
document.addEventListener("keydown", function (event) {
  console.log(event);
})
```

When an arrow key is pressed, the event handler should change the direction of the snake to match. Hint: pressing an arrow key should change the direction but not move the snake… movement happens in `setInterval()`.

When the snake leaves the canvas, it should reappear from the opposite side.

### 2.3: Add the snake food and make the snake grow
As you may remember from PDM 1, this is the trickiest part of the game implementation.

Write a function that will draw some food at a random location on the canvas. I used an orange circle for food in my implementation so that it is visually distinct from the snake. For best results, the food should appear completely inside a grid cell so that when the snake travels over the food, the snake completely covers it—this looks better and makes collision detection easier.

Next, write some code to detect when the head of the snake meets the food (the snake eats the food). When this happens, the food should disappear, the snake’s body should grow by one segment, and a new food should appear at a random location.

Some tips for growing the snake and moving it as it gets longer:

- You probably have one set of x, y coordinates for the snake. Change this to be an array of snake segments (each 20px by 20px square of the body), each with its own x, y coordinates. Use an object (or class) to represent each segment.
- When the snake eats, add a new segment to the end of the array. You will need to work out the coordinates of the new segment based on the snake’s direction.
- When the user changes the axis of the snake’s movement, it should appear to turn a corner. You could achieve this effect with some complicated logic to update the x and y coordinates of each segment of the snake—this is how the snake would move if it were made up of physical blocks. However, a much easier approach to code is to implement movement as follows:
    - Remove the last segment of the snake using the [pop()](https://www.w3schools.com/jsref/jsref_pop.asp) array method.
    - Figure out the coordinates that just the head segment of the snake should be at when it moves. Add a new segment with these coordinates at the front of the array using the [unshift()](https://www.w3schools.com/jsref/jsref_unshift.asp) array method. 
    - Leave the rest of the snake where it is. Take time to make sure you understand why this works! Drawing pictures may help…

## Stage 3 

Library tutorials are available for:
- [How to use Font Awesome icons](#how-to-use-font-awesome-icons)
- [Creating a p5js sketch from scratch (without the PDM 1 template)](#p5js-basics)
- [ThreeJS setup and basic usage](#threejs-setup-and-basic-usage)
- [Create a webcam background With Tensorflow.js](#create-a-webcam-background-with-tensorflow)

You're not expected to do all of them! 

### How to Use Font Awesome Icons
Font Awesome provides high quality free icons that can be used in your HTML and styled with CSS (colour, size). They are very useful for menus, action buttons (e.g. edit, delete) and can also be used as basic sprites. Depending on how you install it, using Font Awesome doesn’t require JS.

#### Setup WITHOUT npm / Vite
The following instructions are an amalgamation of the relevant bits and pieces from [the Font Awesome website](https://fontawesome.com/download). If you'd like to use npm / Vite instead, follow the instructions on the Font Awesome website.

1.	Create a new basic HTML page with linked CSS file.
2.	Go to the [Font Awesome download page](https://fontawesome.com/download) and look for the box titled something like "6.x.x for the web". Click the "Free for Web" button to download a zip of all the files you’ll need plus a bunch that won’t be needed.
3.	Extract the zip. Copy the webfonts and css folders (folders and contents) into the folder with your index.html file. You can move the copied folders into a nested folder if you prefer. Just make sure that webfonts and css folders are in the same folder. 
4.	Open the css folder and delete everything except all.css as it won’t be needed.
5.	Link all.css to your HTML page in the same way as your own css file. If you want to customise the icon appearance (colour, size) link all.css before your own stylesheet.

#### Using a Font Awesome icon in HTML
1.	Go to [the icon reference](https://fontawesome.com/icons) and find an icon you like. You can search by keyword or explore the categories on that page.
2.	Although there are lots of free icons, some are Pro only (i.e. paid). On the icon search result page, there is an option to show only free icons. Identify a free icon that you like and click on it to get usage instructions.
3.	Clicking on an icon opens a modal window. In the top right, you will see some HTML code (an `<i>` element with classes). Copy this code and paste it into your index.html file. You will see the icon displayed when you load your HTML file using Live Preview.

#### Styling a Font Awesome icon
The classes supplied by Font Awesome are required to display the icon. Icons will usually have two classes. Do not delete or rename either of these classes. Do not attempt to edit all.css either.

To style a Font Awesome icon add your own CSS in your own stylesheet. You can add your own custom classes to a Font Awesome icon by adding the class name to the end of the class list on the `<i>` element. You can also give icons an id or apply style rules to all icons by selecting the `i` tag in your CSS.

The icon is rendered as a text element, so you can change colour by using the `color` property as you would for text. You can also change the size using the `font-size` property. Other properties you can change include `margin`, `padding`, and `position`.

Icons can be used anywhere you can use a span element: inside paragraphs, headings, links, list items, buttons, and more.

### p5js Basics
In PDM1 and 2, we gave you a template to work with. This section will explain how to set up p5.js from scratch.

#### Setup
1.	Create a new basic HTML page and linked CSS and JS files.
2.	Go to [https://p5js.org/download/](https://p5js.org/download/), scroll down to the "Single Files" section and look for the CDN button.
3.	Click the CDN button. This will take you to a new page. Under the Version dropdown menu, you will see several URLs. You want the first one, which ends in min.js. Click the icon that looks a like an empty HTML element: `</>` . This will add to your clipboard a complete `<script>` element with the `src` already populated. Paste this into your HTML file at the end of `<body>` but above the `<script>` element that links your own JS file.

#### Create a basic sketch
The following steps are the same as the official [Getting Started tutorial](https://p5js.org/get-started/) but they have been adapted for use in VS Code rather than the p5js web editor.

Put the following code into your main.js file:
```Javascript
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}
```
Refresh your memory of p5.js by trying these tasks:
- The sketch background is greyscale. Make it an RGB colour instead.
- Draw a circle at 50, 50 and give it a radius of 80 pixels. 
- Give the circle a different fill colour when the mouse is pressed. 
- Modify your code so that the circle follows the mouse.

#### Using HTML input controls with your p5js sketch
You used p5.js's version of input controls (buttons etc.) last year. Because p5js runs in the browser, you can also use standard HTML input elements to add more user control to your sketch.

Start by adding a standard HTML button that will appear above the sketch, rather than inside it. In your HTML file, add an HTML button input and give it an `id` and a value. The button will be used to change the background colour of your sketch. If you check the output, you should see that the button appears above the sketch.

In main.js, look at how and where the background colour is set. It is called with a hard-coded colour value every time `draw()` is called. As with regular Processing, if you want to be able to change the colour in response to user input, you will need a global variable to store the current colour and pass that variable to `background()` instead of the hard-coded value. In Processing, you could call some built-in functions like `color()` where global variables are initialised (outside `setup()`). In p5js, however, these functions can only be called inside `setup()` or after `setup()` has run. To create and use a variable to set the background colour, do the following:

- Declare the variable at the top of main.js. Don’t give it a value.

```javascript
let bgColour;
```
- Anywhere in `setup()`, assign the variable a colour of your choosing using the `color()` function. E.g.:
```javascript
bgColour = color(255, 255, 0);
```
- Replace the hard-coded numbers in `background()` with the variable name:
```javascript
background(bgColour); 
```
Check that everything works before moving on. 

Next, add an event listener to your button that will give your background colour variable a new value when the button is clicked. You can call the p5js `color()` function inside your event handler. You can also use an anonymous function (traditional or arrow) to implement the event handler as you would in vanilla JS.

p5js always adds the canvas to the end of the `<body>`, after anything else that’s rendered. Take a look at [this page](https://github.com/processing/p5.js/wiki/Positioning-your-canvas) for guidance on how to position the canvas elsewhere.

Try using CSS to style the canvas in other ways.

### ThreeJS setup and basic usage
The recommended approach for working with this library is to use Node.js and Vite. 

To install with Node.js and Vite, create a new Vite project as shown last practical. Then,
1. Open the project in VS Code
2. Run this terminal command to install ThreeJS: `npm install three`
3. To check it has installed, open `package.json` and look for "three" in the "dependencies".

If you would like to use it without dealing with Vite, follow the steps below to set it up using a CDN instead.
1. Create a new basic HTML page and linked CSS and JS files.
2. In your HTML file, find the `<script>` tag that links your JS file. Add the attribute `type="module"` to the script tag (modules will be explained in week 11). This allows you to use the import syntax as shown in the ThreeJS documentation.
3. Still in the HTML file, add the following code at the very end of the `<head>` element:
```javascript
<script type="importmap">
    {
        "imports": {
            "three": "https://cdn.jsdelivr.net/npm/three@0.174.0/build/three.module.js",
            "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.174.0/examples/jsm/"
        }
    }
</script>
```
This `<script>` element contains a JSON object that enables ThreeJS and common "addons" to be imported in your JavaScript file from CDN links. Notice that the values of the two properies that specify URLs contain "@0.174.0". This specifies the version of ThreeJS to import. 0.174.0 is the current version at the time of writing. You should change this value if you would like to use a different version.

No matter which installation approach you took, ThreeJS is now ready for use. Try following the [Create a Scene](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene) tutorial in the ThreeJS docs to learn the basic concepts. The completed code is available in the sample solution for this practical, which is already available in the Lecture-Examples repo. Look for the threejsBasicSetup folder.


#### Using 3D models created elsewhere
ThreeJS recommends that you use models exported in the glTF format. This format is supported by Blender. You can also find free 3D models on [sketchfab.com](https://sketchfab.com/).

In order to load glTF models, you will need to use the ThreeJS addon, `GLTFLoader`. The [official docmentation](https://threejs.org/docs/#examples/en/loaders/GLTFLoader) is somewhat helpful but I suggest you take a look at the threejsImportedModel example in the practical sample solutions.

General troubleshooting tips:
- Always check for JavaScript errors in the console first
- If you can't see your model and there are no errors, try loading your model in [this ThreeJS powered viewer](https://gltf-viewer.donmccurdy.com/). 
    - If you can't see the model in the viewer, then there is a problem with your model. Check its format and textures etc. 
    - If you *can* see the model in the viewer, then the problem is likely to do with how your scene is set up in your code. This is usually to do with the lighting, camera position, model position, or model size. It is recommended that you start with the threejsImportedModel sample code and modify it to work with your own model. If you are not using the sample code, make sure you have the scene background colour set to white or light grey. Texture and lighting problems usually result in the model appearing black so you won't be able to see it on a black background!

#### Adding controls
ThreeJS provides a number of different options for allowing the user to control the camera. The threejsImportedModel sample solution shows how to setup and use the [OrbitControls](https://threejs.org/docs/index.html#examples/en/controls/OrbitControls). Try some of the others listed in the docs. If the documentation is a little sparse, look for examples that use the controller of interest. Relevant examples are linked from the documentation pages.

### Create a Webcam Background With TensorFlow
TensorFlow.js is a Google machine learning library that allows you to embed some AI capabilities in your web applications. Taking full advantage of TensorFlow.js requires advanced technical skills and knowledge of machine learning fundamentals but this tutorial is intended to be approachable with just the JS covered in this module. 

Most video conferencing apps allow users to blur their background or replace it with an image. This is done using a computer vision technique called segmentation, which separates background from objects of interest in an image. In this tutorial, you will use body segmentation, where the object of interest is a human, to create an image background. If you don’t have a webcam, you can still do the setup and the body segmentation sections of the tutorial. A webcam is required for the final section.

#### Setup
1.	Create a new basic HTML page and linked CSS and JS files.
2.	Add TensorFlow.js to your HTML file by copying the `<script>` element found under "Usage via script tag" on [this page](https://www.tensorflow.org/js/tutorials/setup) and pasting it just above the `<script>` element that connects main.js.
3.	TensorFlow.js provides pre-trained models for a number of tasks. To use a pre-trained model, you need to import it alongside the library itself. For this tutorial, you will need two pre-trained models for body segmentation. Import these models by copying the following script tags into your HTML between the script tag from step 2 and the script tag linking main.js:
```html
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation"></script>
<script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/body-segmentation@1.0.1"> </script>
```
#### Body segmentation
The following steps are based on [this TensorFlow.js example](https://github.com/tensorflow/tfjs-models/tree/master/body-segmentation) but they have been heavily customised.

#### Complete the HTML page
1.	Copy the images folder in this repo into the folder that contains your HTML.
2.	In your index.html file, add an image element above the scripts and give it the `id` "image". Use the image called "StockSnap_5TCGRN12WA.jpg" as the image source. You will eventually use the body segmentation model to separate the person in the foreground of the image from the wall in the background.
3.	Below the image but above the scripts, add a canvas element, give it the `id` "canvas" and set its width and height to match the image (800px by 533px). The segmented image will be displayed on the canvas later on. Open index.html in Chrome using Live Preview and check that you see the image in your browser. 
4.	Give the canvas a CSS border or background colour so that you can see it on the screen even when nothing is drawn on it.

#### Create global variables in your JS file
1.	In your JS file, create global variables to store the image element, the canvas, and its context. When getting the canvas context, you will need to pass a second argument that will improve performance when the webcam is used. Here is the code:

```javascript
const image = document.getElementById("image");
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d", {willReadFrequently: true});
```
2.	Create another variable that will store a reference to TensorFlow’s bodySegmentation model. Copy this code as is:
```javascript
const model = bodySegmentation.SupportedModels.MediaPipeSelfieSegmentation;
```
3.	Add the following variable, a JSON object that stores configuration information for the model, such as which specific model to use:
```javascript
const segmenterConfig = {
  runtime: 'mediapipe', 
  solutionPath: 'https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation',
  modelType: 'general'
}
```
#### Segment the image
Using a machine learning model for classification typically involves three steps:
1. Loading the classifier, which is essentially a file or data structure containing a lot of numerical data
2. Passing some new data to the classifier and waiting for it to respond with a prediction or label
3. Doing something with the classifier output.

As models are generally very large, steps 1 and 2 can be slow so they are usually carried out using *asynchronous* operations, similar to the asynchronous operations to fetch data from a web API.

You will now write functions to carry out each stage.
1. Outline a function called `setup`. It should not have any parameters. This function will load the classifier, which we'll refer to as the "segmenter".
2. Outline a function called `segment`. It should have one parameter called `segmenter`. This function will pass data to the classifier and wait for a response.
3. Call `setup()` at the bottom of your script file.
4. Add the following code inside your `setup` function:

```javascript
bodySegmentation.createSegmenter(model, segmenterConfig)
                .then(function (segmenter) {
                    segment(segmenter);
                });
```
This code calls an asynchronous method on the TensorFlow.js bodySegmentation model to create a new classifier with the configuration created early. As it is asynchronous, we call `then()` to wait for the classifier. The classifier is passed to the `segmenter` parameter in the anonymous function in `then()`. At this point, we know the classifier is fully loaded so we call `segment()` and pass it the classifier.

5. Add the following code inside your `segment()` function:

```javascript
// Find the person in the image
segmenter.segmentPeople(image)
         .then(function (people) {
                // The colour to set pixels in the foreground (person, transparent)
                const foreground = {r: 0, g: 0, b: 0, a: 0};
                // The colour to set pixels in the background (wall, green)
                const background = {r: 0, g: 255, b: 0, a: 255};
                // Create a mask with the given colours (green background, transparent person)
                bodySegmentation.toBinaryMask(people, foreground, background)
                                .then(function (maskImage) {
                                      // Draw the mask on the canvas
                                      ctx.putImageData(maskImage, 0, 0);
                                });
          });
```
Check the output of your code in Chrome. You should see the original image at the top of the page and the segmented mask on the canvas. The segmented mask should look like this:
![segmented_image](https://user-images.githubusercontent.com/50892754/222404363-b99f843a-2ff9-4d62-9e0c-844e3aae6fc1.png)

#### Remove the background
As the background is now a solid green, it can easily be removed by iterating through the images pixels and setting any green pixels to transparent.

1. Write a function called `isPixelGreen` that takes four parameters, `r`, `g`, `b`, and `a`, representing the colour channels in an image, including alpha. The function should return `true` if the `r` and `b` are 0 and `g` and `a` are 255.
2. Outline a function called `drawImageWithoutBackground` that takes two parameters, `image`, and `maskData`. `image` represents the original image, and `maskData` represents the segmented mask returned by the classifier.
3. In your `segment()` function, replace the line `ctx.putImageData(maskImage, 0, 0);` with a call to the new function: `drawImageWithoutBackground(image, maskImage);`
4. Add the following code inside `drawImageWithoutBackground`:

```javascript
// Draw the image on the canvas
ctx.drawImage(image, 0, 0);
// Read the image data from the canvas, a JavaScript ImageData object
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
// Get the pixel array from the image data
const imagePixels = imageData.data;
// Get the pixel array from the mask data
const maskPixels = maskData.data;
// Iterate through the mask pixels. If the mask pixel is green, turn the corresponding mask pixel transparent
for (let i = 0; i < maskPixels.length; i+=4) {
  if (isPixelGreen(maskPixels[i], maskPixels[i+1], maskPixels[i+2], maskPixels[i+3])) {
    imagePixels[i+3] = 0; // make the image pixel transparent by setting the alphas 
  }
}
// Draw the updated image pixels to the canvas
ctx.putImageData(imageData, 0, 0);
```
There's a lot going on in this code. In order to modify the pixels in an image, we need to get its `ImageData` object, which has a property called `data`, a 1D array representing all the pixels. The `toBinaryData()` method used in the `segment()` function returns an `ImageData` object, which will be passed to `drawImageWithoutBackground()` as the `maskData` parameter, so we already have access to the individual pixels in the segmented mask. To get the pixels from the raw image, we need to draw it on the canvas using `ctx.drawImage()`, then get its `ImageData` object using `ctx.getImageData()`.

The key part of the code above is the for loop. Conceptually, the for loop iterates through the *mask* pixels to find green pixels then it sets the alpha channel of the corresponding *image* pixel to transparent, thus removing the background and leaving the person. Notice that the for loop jumps 4 pixels on each iteration. This is because the array stores four values for each pixel, representing its colour channels (R, G, B, A). You can read more about the pixel array [in the docs](https://developer.mozilla.org/en-US/docs/Web/API/ImageData/data).

Once the background of the image is removed, the pixels are drawn to the canvas using `ctx.putImageData()`.

The output on the canvas should look like this:
![background_removed](https://user-images.githubusercontent.com/50892754/222404479-6b0c4f92-6982-4c1d-9788-1c4a148b293a.png)

#### Add a new background
1. In your CSS file, create a rule declaration for your canvas if you haven't done so already.
2. Set the [`background-image`](https://www.w3schools.com/cssref/pr_background-image.php) property of the canvas. You can use the business-people-pointing-1557758791FVV.jpg image that should already be in the images folder, or you can use an image of your choosing. The image should be about the same size as the canvas (don't worry about an exact fit just yet... your webcam image is likely to be a different size).
3. Depending on the size of your background image, you may see some repeating, or other undesirable effects. You may wish to look up some of the CSS properties for controlling a background, such as [`background-repeat`](https://www.w3schools.com/cssref/pr_background-repeat.php) and [`background-size`](https://www.w3schools.com/cssref/css3_pr_background-size.php). Here is my CSS for the canvas:

```css
canvas {
    background-image: url('images/business-people-pointing-1557758791FVV.jpg');
    background-repeat: no-repeat;
    background-size: contain;
    border: solid 1px;
}
```
In your browser, the canvas should now look like this:
![new_background](https://user-images.githubusercontent.com/50892754/222407799-06e9d5ab-3d21-4e36-bd99-b27d30ea86e9.png)

Here is the complete JavaScript at this point in the tutorial:
```javascript
const image = document.getElementById("image");
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d", {willReadFrequently: true});

const model = bodySegmentation.SupportedModels.MediaPipeSelfieSegmentation;
const segmenterConfig = {
  runtime: 'mediapipe', 
  solutionPath: 'https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation',
  modelType: 'general'
}

function setup {
    bodySegmentation.createSegmenter(model, segmenterConfig)
                    .then(function (segmenter) {
                           segment(segmenter);
                    });   
}

function segment(segmenter) {
    segmenter.segmentPeople(image)
             .then(function (people) {
                     const foreground = {r: 0, g: 0, b: 0, a: 0};
                     const background = {r: 0, g: 255, b: 0, a: 255};
                     bodySegmentation.toBinaryMask(people, foreground, background)
                                     .then(function (maskImage) {
                                              drawImageWithoutBackground(image, maskImage);
                                     });
             });
}

function isPixelGreen(r, g, b, a) {
    return r === 0 && g === 255 && b === 0 && a === 255;
}

function drawImageWithoutBackground(image, maskData) {
    ctx.drawImage(image, 0, 0);
    const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
    const imagePixels = imageData.data;
    const maskPixels = maskData.data;
    for (let i = 0; i < maskPixels.length; i+=4) {
        if (isPixelGreen(maskPixels[i], maskPixels[i+1], maskPixels[i+2], maskPixels[i+3])) {
            imagePixels[i+3] = 0;
        }
    }
    ctx.putImageData(imageData, 0, 0);
}

setup(); 
```
#### Capturing and segmenting your webcam video
From this point on, a webcam is required.

You are done with TensorFlow.js. For the next stage of the tutorial, you will work with JavaScript's built in [MediaDevices interface](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices), which enables you to capture input from the user's webcam and microphone.

1. In your HTML file, remove the image element and replace it with a `<video>` element as shown below. This will temporarily break your JavaScript.
```html
<video id="webcam"></video>
```
2. In your JS file, delete the call to `setup()` at the bottom of the script but leave the function as it is. Instead of calling the function on page load, you will request access to the user's webcam and wait for the video stream to start before calling `setup()`.
3. Delete the global `image` variable and replace it with a global `video` variable, which references the video element:

```javascript
const video = document.getElementById("webcam");
```
4. Go through your script, and replace any usages of the `image` variable with `video`. There should just be two instances, which you can find in `segment()` .
5. At the bottom of the JS file, add the following code:

```javascript
navigator.mediaDevices
         .getUserMedia({
            audio: false,
            video: { width: canvas.width, height: canvas.height }
          })
          .then(function (mediaStream) {
              video.srcObject = mediaStream;
          })
          .catch(function (err) {
              console.error("something went wrong with the webcam", err);
          });
```
This code uses the built-in `navigator.mediaDevices` object to request access to the webcam using the `getUserMedia()` method. The argument passed to this method is a JSON object configuring the request—audio is turned off and the requested width and height match the canvas width and height. Getting access to the camera is an asynchronous operation as the code has to prompt the user for permission (first time only) then wait for the camera to open and the stream to become available. When the stream is available, it is passed to `then()` as the `mediaStream` parameter. The next line sets the webcam stream as the source of the HTML `video` element. As there are lots of things that could go wrong, such as the user refusing permission, there is an `error()` method to print out a message if needed.

6. If you check your browser now, you will probably see a big empty space at the top of the page and no video. To play the video and start segmentation on the webcam stream, add an event listener that will wait for the video to load:

```javascript
video.addEventListener("loadedmetadata", function () {
    video.play(); 
    setup();
});
```
You should see the webcam video playing at the top of your browser window, and on the canvas, a still image of one frame of your webcam stream with the background image you added earlier.

7. The very last thing to do is to repeatedly call `segment()` so that the canvas shows video rather than a still image. In `segment()`, add the following line immediately after `drawImageWithoutBackground(video, maskImage);`:

```javascript
setTimeout(function () {
    segment(segmenter)
}, 15);
```
If you would like to hide the raw webcam stream, you can set its CSS `position` property to `absolute` and its `left` property to something like `-100vw`.

Here is the complete JavaScript code:
```javascript
const canvas = document.getElementById("canvas");
const video = document.getElementById("webcam");
const ctx = canvas.getContext("2d", {willReadFrequently: true});


const model = bodySegmentation.SupportedModels.MediaPipeSelfieSegmentation;
const segmenterConfig = {
  runtime: 'mediapipe', 
  solutionPath: 'https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation',
  modelType: 'general'
}

function setup() {
    bodySegmentation.createSegmenter(model, segmenterConfig)
        .then(function (segmenter) {
            segment(segmenter);
        });
    
}

function segment(segmenter) {
    segmenter.segmentPeople(video)
        .then(function (people) {
            const foreground = {r: 0, g: 0, b: 0, a: 0};
            const background = {r: 0, g: 255, b: 0, a: 255};
            bodySegmentation.toBinaryMask(people, foreground, background)
                .then(function (maskImage) {
                    drawImageWithoutBackground(video, maskImage);
                    setTimeout(function () {
                        segment(segmenter)
                    }, 15);
                });
        });
}

function isPixelGreen(r, g, b, a) {
    return r === 0 && g === 255 && b === 0 && a === 255;
}


function drawImageWithoutBackground(image, maskData) {
    ctx.drawImage(image, 0, 0);
    const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
    const imagePixels = imageData.data;
    const maskPixels = maskData.data;
    for (let i = 0; i < maskPixels.length; i+=4) {
        if (isPixelGreen(maskPixels[i], maskPixels[i+1], maskPixels[i+2], maskPixels[i+3])) {
            imagePixels[i+3] = 0; // make the image pixel transparent
        }
    }
    ctx.putImageData(imageData, 0, 0);
}

video.addEventListener("loadedmetadata", function () {
    video.play(); 
    setup();
});

navigator.mediaDevices
    .getUserMedia({
        audio: false,
        video: { width: canvas.width, height: canvas.height }
    })
    .then(function (mediaStream) {
        video.srcObject = mediaStream;
    })
    .catch(function (err) {
        console.error("Something went wrong with the webcam", err);
    });

```
### Extension
You are done with the tutorial but can go further by applying your knowledge of JavaScript to improve the user experience. For example, you could:
- Add a button that allows the user to toggle the webcam on and off (you will need to do some research on how to play / pause the video, and listen for play / pause events)
- Add a button that allows the user to toggle between the raw video background and the background image.
- Allow the user to change the background image. The "file" input type will be useful for this. You should also do some research on how to read an image file as a data URL.