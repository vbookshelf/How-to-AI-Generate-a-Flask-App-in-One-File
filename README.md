# How to use AI to generate a complete Flask Web App in just One File

> YouTube Video:<br>
> How to use AI to Generate a Single-File Flask Web App that Runs Locally<br>
> https://youtu.be/MSDjeUhunGA?si=TOZVgNdC5VQ0ff_8

<br>

Use a code example plus AI to quickly build flask apps that run locally. Create privacy-focused web apps without the complexity of a multi-file project structure. Python, HTML, CSS and JS code are all in one python file.

This project shows how to prompt AI models to create a flask app that combines a nice interactive UI running in the browser with the power of a Python backend. This approach is ideal for domain experts who want to use Gemini, ChatGPT and Claude to create digital tools/small-apps that run locally, without an internet connection.

<img src="https://github.com/vbookshelf/Single-File-Flask-Web-App/blob/main/images/image2.png" alt="Doctor working on a laptop" height="350">

The user provides a code example as a pattern to follow and then asks an LLM to generate the code for the app. The user then copies this code and pastes it into into a python file. Then the user runs the python file on their computer by typing a command in the console. When the file is running the app can be accessed from a web browser. This workflow makes it easy for non-programmers to create custom tools that need both a frontend and a backend.

Features:
- All the beauty of HTML, CSS and JS
- All the power of Python and its packages
- All running locally
- No complicated project folder structure
- Easy to AI generate and modify because all the code is on one page

### Example 
A doctor wants to create an app that uses an Ollama model to extract patient data from written records. For privacy reasons this app would need to run locally. Also, it would need a UI to ingest the written records and a Python backend to pre-process the data and to run the model. 

To create this app the doctor would simply need to describe it to an LLM like ChatGPT or Gemini. The doctor would also need to give the code, shown below, to the LLM as an example of the architecture that should be used. The LLM would then generate a flask app in one file. The doctor would then run that app like a normal Python file (% uv run python app.py) - no need for Flask-specific commands.

<br>
<br>

<hr>

### The example code

### UI: Browser based user interface
<img src="https://github.com/vbookshelf/One-Page-Desktop-Flask-Web-App-Template/blob/main/images/example-app.png" alt="Simple chat app running in web browser" height="400">

### Code: Python, HTML, CSS and JS in one file (app.py)

```

from flask import Flask, render_template_string, request, jsonify

app = Flask(__name__)

# Simple inline HTML template
HTML = """
<!DOCTYPE html>
<html>
<head>
  <title>Flask Chat</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    #chat { border: 1px solid #ccc; padding: 10px; height: 300px; overflow-y: auto; }
    #input { margin-top: 10px; }
  </style>
</head>
<body>
  <h2>Flask Chat</h2>
  <div id="chat"></div>
  <div id="input">
    <input type="text" id="message" placeholder="Type a message" size="40"/>
    <button onclick="sendMessage()">Send</button>
  </div>

  
<script>

// Function to handle sending a message to the 
// python (Flask) backend and receiving a response.

async function sendMessage() {

  // Get the message text from the input box
  const msgInput = document.getElementById("message");
  const msg = msgInput.value;

  // If the message is empty or just spaces, do nothing
  if (!msg.trim()) return;

  // Get the chat display area
  const chat = document.getElementById("chat");

  // Show the user's message in the chat
  chat.innerHTML += `<div><b>You:</b> ${msg}</div>`;

  // Clear the input box
  msgInput.value = "";

  try {
    // Send the message to the server
    const response = await fetch("/send", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ message: msg }) // Send message in JSON format
    });

    // Convert the server's response into JSON
    const data = await response.json();

    // Show the bot's reply in the chat
    chat.innerHTML += `<div><b>Bot:</b> ${data.reply}</div>`;

    // Scroll chat to the bottom so the latest message is visible
    chat.scrollTop = chat.scrollHeight;

  } catch (error) {
    // If something goes wrong, show an error message in the chat
    console.error("Error:", error);
    chat.innerHTML += `<div><b>Bot:</b> Sorry, something went wrong.</div>`;
  }
  
}

</script>
</body>
</html>
"""

@app.route("/")
def index():
    return render_template_string(HTML)

@app.route("/send", methods=["POST"])
def send():
    user_msg = request.json.get("message", "")
    reply = f"Echo: {user_msg}"  # Fake bot reply
    return jsonify({"reply": reply})

if __name__ == "__main__":
    app.run(debug=True)

```

<hr>

<br>

## How to run the example code
Please ensure that you have UV installed. UV is a Python package manager that's a faster alternative to Pip.<br>

```
These are the steps for Mac.

1. Download the project folder, unzip it and place it on your desktop.
In this repo the project folder is named: one-page-flask-app
Then open your command line console.
The instructions that follow should be typed on the command line. 
There’s no need to type the % symbol.

2. % cd Desktop

3. % cd one-page-flask-app

4. Initialize a python 3.10 environment inside the folder
(This step only needs to be done the first time)
% uv init --python 3.10

5. Install flask
(This step only needs to be done the first time)
% uv add flask

6. Launch the app
% uv run python app.py

7. Copy the url that gets printed out (e.g. http://127.0.0.1:5000/)

8. Paste the url into your web browser and press Enter. The app will launch in the browser. 

9. To stop the app type ctrl C in the console.
```
<br>

## Test the idea

To test this idea let's use Google Gemini to build a chat interface that we can use to chat with an Ollama model locally.

Please enter the prompt below into Gemini: https://gemini.google.com (The gemini flash model worked fine during my test.)<br>
Remember to select "Canvas." Attach the app.py file from this repo to the prompt as an example for the LLM to follow. You'll need to have Ollama already installed. Also, you'll need to have already downloaded the gemma3:4b ollama model.

> Prompt:

> *I want to create a chat app that I can run locally. I want to chat with the gemma3:4b Ollama model. I have already downloaded the model. Please use the design approach shown in the attached example where all the code is in one file. Please use the Ollama Python package and not the Ollama API.*

<br>

- Gemini will generate the code. 
- Copy the code and paste the code into a python file. 
- Name the file ```app.py```
- Put the file into the ```one-page-flask-app``` project folder.

Now follow the steps shown above to run the code. 

- You will get an error message in the console: ModuleNotFoundError: No module named 'ollama'
- Install the ollama python package by typing the following in the console: ```uv add ollama```

You should now have a working chat app running in your browser. Note the the responses may be slow depending on how fast your computer is. Therefore, during the chat, allow some time for the model to respond.
<br>

## Notes

- I had good success using Google Gemini 2.5 Pro and GPT-5 when creating a one-file app.
- I tried Firebase Studio but I couldn't get it to create a simple one-file app that could run locally. It seems that this product is geared towards creating projects with an architecture that makes them easy to auto deploy on Google servers.

## Conclusion

For so long the power to create software was held by highly skilled programmers. But that barrier is starting to dissolve. Now, by using this simple approach, anyone can build their own custom digital tools and run them offline.

<br>
<br>
