# Gradio: Create ML Web Apps UI with Python

## What Does Gradio Do?

One of the best ways to share your machine learning model, API, or data science workflow with others is to create an interactive app that allows your users or colleagues to try out the demo in their browsers.

Gradio allows you to build demos and share them, all in Python. And usually in just a few lines of code! So let's get started.

## Hello, World

To get Gradio running with a simple "Hello, World" example, follow these three steps:

* Install Gradio using pip:
    

```python
pip install gradio
```

* Run the code below as a Python script or in a Jupyter Notebook (or Google Colab):
    

```python
import gradio as gr

def greet(name):
    return "Hello " + name + "!"

demo = gr.Interface(fn=greet, inputs="text", outputs="text")

demo.launch()
```

* The demo below will appear automatically within the Jupyter Notebook or pop in a browser on `http://localhost:7860` if running from a script:
    

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668849998035/UI6kGpWvF.png align="left")

## The Interface Class

You'll notice that in order to make the demo, we created a gradio.Interface. This Interface class can wrap any Python function with a user interface. In the example above, we saw a simple text-based function. Still, the function could be anything from a music generator to a tax calculator to the prediction function of a pre-trained machine learning model.

The core Interface class is initialized with three required parameters:

### `fn:` the function to wrap a UI around

inputs\`: which component(s) to use for the input (e.g. "text", "image" or "audio") outputs: which component(s) to use for the output (e.g. "text", "image" or "label") Let's take a closer look at these components used to provide input and output.

### Components Attributes

We saw some simple Textbox components in the previous examples, but what if you want to change how the UI components look or behave?

Let's say you want to customize the input text field — for example, you wanted it to be larger and have a text placeholder. If we use the actual class for Textbox instead of using the string shortcut, you have access to much more customizability through component attributes.

```python
import gradio as gr

def greet(name):
    return "Hello " + name + "!"

demo = gr.Interface(
    fn=greet,
    inputs=gr.Textbox(lines=2, placeholder="Name Here..."),
    outputs="text",
)
demo.launch()
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668850151218/YGNKUqsLd.png align="left")

## Multiple Input and Output Components

Suppose you had a more complex function, with multiple inputs and outputs. In the example below, we define a function that takes a string, boolean, and number, and returns a string and number. Take a look at how you pass a list of input and output components.

```python
import gradio as gr

def greet(name, is_morning, temperature):
    salutation = "Good morning" if is_morning else "Good evening"
    greeting = f"{salutation} {name}. It is {temperature} degrees today"
    celsius = (temperature - 32) * 5 / 9
    return greeting, round(celsius, 2)

demo = gr.Interface(
    fn=greet,
    inputs=["text", "checkbox", gr.Slider(0, 100)],
    outputs=["text", "number"],
)
demo.launch()
```

## Conclusion

In the end, I would say Gradio is a good tool for testing your ML application, Obviously, you won't use this in production. In Production, you will have to create an API and then use that API in your frontend application