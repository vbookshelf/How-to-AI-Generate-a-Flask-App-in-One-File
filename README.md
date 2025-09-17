# One Page Flask Web App Template

Create a simple, local, and privacy-focused web app without the complexity of a multi-file project structure.

This project shows how to create a one-page app the combines a nice UI running in the browser with the power of a Python backend. This approach is ideal for those who are using Gemini, ChatGPT and Claude to create digital tools/small-apps that need to run locally.

<img src="https://github.com/vbookshelf/One-Page-Flask-Web-App-Template/blob/main/images/one-page.png" alt="Programmer holding up a page" height="350">

A user can provide the template as a starting point and ask an LLM to generate the rest of the code for a specific task, such as a local chat application or a data extraction tool. This makes it easy for non-programmers to create their own custom tools that need both a frontend and a backend.

Features:
- All the beauty of HTML, CSS and JS
- All the power of Python and its packages
- All running locally
- No complicated project folder structure
- Easy to modify by vibe coding because all the code is on one page

### Example 
A doctor wants to create an app that uses an Ollama model to extract patient data from written records. For privacy reasons this app would need to run locally. Also, it would need a UI to ingest the written records and a Python backend to pre-process the data and to run the model. 

To create this app the doctor would simply need to describe it to an LLM like ChatGPT or Gemini. The doctor would also need to give the one-page code, shown below, to the LLM as an example of the approach that should be used. The LLM would then generate a one-page flask app. The doctor would then run that app like a normal Python file (% uv run python app.py) - no need for Flask-specific commands.

<br>
<br>

<hr>

### Simple example code

### UI: Browser based user interface
<img src="https://github.com/vbookshelf/One-Page-Desktop-Flask-Web-App-Template/blob/main/images/example-app.png" alt="Simple chat app running in web browser" height="400">

### Code: Python, HTML, CSS and JS on one page

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

## How to run this code
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

## Notes
- I had good success using Google Gemini 2.5 Pro and GPT-5 when creating the one page app.
- I tried Firebase Studio but I couldn't get it to create a simple one page app that could run locally. It seems that this product is geared towards creating projects with an architecture that makes them easy to auto deploy on Google servers.
