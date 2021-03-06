# Tutorial: Text to Image (AttnGAN and p5.js)

🎉Community Contribution by [Matthew Kenney](http://matthewkenney.site/)

This tutorial will review how to connect an AttnGAN model in RunwayML to a sketch in [p5.js](https://p5js.org/). AttnGAN is a generative adversarial network model that takes in words or phrases, and outputs images like the words it's queried against. This model is great for creative image generation, and can be included to enhance our p5.js sketches. In this tutorial, we'll use HTTP to connect p5.js to RunwayML.

### Requirements

- [RunwayML](https://runwayml.com/)
- [p5.js](https://p5js.org/download/)
- [p5.dom library](https://p5js.org/reference/#/libraries/p5.dom)

### Code
Find the code for this tutorial [here](https://github.com/runwayml/p5js/tree/master/AttnGAN).

### Step 1

![AttnGan with p5.js with RunwayML, step 1](assets/images/tutorials/tutorial_p5_attngan/selection.png)

Search for AttnGAN in RunwayML. The model should be under the **Image Generation** section of the interface. Select it.

### Step 2

![AttnGan with p5.js with RunwayML, step 2](assets/images/tutorials/tutorial_p5_attngan/info.png)

You should see a page with further information about the model. In the upper right hand corner, select the button **Add To Workspace** and then **New Workspace**. Create a new workspace by giving it any name you want.

### Step 3

![AttnGan with p5.js with RunwayML, step 3](assets/images/tutorials/tutorial_p5_attngan/interface.png)

You should then see an interface like the one pictured above. AttnGAN requires a GPU to run. In the bottom right hand corner of the interface, click **Run Remotely** to start the model running on a remote GPU.

### Step 4

![AttnGan with p5.js with RunwayML, step 4](assets/images/tutorials/tutorial_p5_attngan/port.png)

In the upper right hand corner, select **Network** and click on **HTTP**. The most important step for this sketch is ensuring that the sketch is connecting to RunwayML and able to communicate with it. This time, we'll use HTTP to connect our p5 sketch to RunwayML. RunwayML will expect requests on port 8000 (image above)

### Step 5

![AttnGan with p5.js with RunwayML, step 5](assets/images/tutorials/tutorial_p5_attngan/code.png)

Next we'll want to run the code. The code can be found [at this link](https://github.com/runwayml/p5js/tree/master/AttnGAN). To run the code from terminal, `cd` into the folder and run `$ python -m SimpleHTTPServer 8001`  for Python 2  or  `python3 -m http.server 8001` if you are using Python 3. Navigate to `localhost:8001` in your browser. Be sure to use the `8001` setting as Python simple server will default to port 8000, which will conflict with RunwayML.

### Step 6

Lets take a look at the code to get a sense of what's going on. The code will be in the `attngan.js` file inside of the root folder.

### The Code in Detail (HTML)
In your html, be sure to include the [p5.js library](https://p5js.org/download/), and the [p5.js DOM library](https://p5js.org/reference/#/libraries/p5.dom). In this example, I have also included [Bootstrap](https://getbootstrap.com/) (optional) to make the layout nicer. In the body of the `index.html` we've included a div with `id="input"`. We'll attach our text input box to this div in our sketch.

### The Code in Detail (JavaScript)
We've commented the JavaScript code for your reference, but there are a few important parts to pay attention to. 

First, we'll define the url we'd like to post to. RunwayML expects post requests to `/query` on port 8000.

```js
let url = 'http://localhost:8000/query';
```

Next, we'll define an input field to allow the user to input some text in our interface. We'll use `createInput` from the p5.js DOM library. We'll give this input a position on the page, and a callback function `sendText`, which we'll define later in our code. We'll also add a `form-control` class to  make it look nice, and we'll set the parent to `input`. Setting the parent to `input` will attach our input form to the div with `id="input"` that we've defined in the html. This will give us more control of where the form is on the page.

```js
    //create a text input with P5 Dom Library
    input_text = createInput('');
    input_text.position(0, 100);
    input_text.input(sendText);
    input_text.addClass("form-control");
    input_text.parent("input")
```

Next, we'll define the `sendText` callback function mentioned above. This function will take the text that is entered into the input field and post it to the url we've defined above. RunwayML is expecting a json object with a key of `caption` and a value that is the text value we would like to query. 

```js
function sendText() {
  //send the text to RunwayML via HTTP. The this.value is the value
  //of the input text
    console.log('you are typing: ', this.value());

    postData = { caption: this.value()};

  httpPost(url, 'json', postData, function(result) {
    newDrawing(result)
  });
}
```

### Step 7

![Skeleton tracking in Processing with RunwayML, step  8](assets/images/tutorials/tutorial_p5_attngan/header.png)

If all works well, your sketch should look something like the image above! As you type in a word or phrase, you should see the image change with each keystroke. This setup will make a post request after each keystroke in the textbox. If you'd like to send the text only after the phrase is completed, try adding a button! 

### Summary

In this tutorial we reviewed how to set up AttnGAN with p5.js, communicating to AttnGAN in RunwayML via HTTP. From here, you should be able to use the output in interesting ways in your sketch. We're looking forward to seeing how you extend this example!