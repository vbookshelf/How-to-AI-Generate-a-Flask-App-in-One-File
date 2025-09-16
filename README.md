# One Page Desktop Flask Web App Template
A simple example that shows how to create a one-page desktop app the combines a nice UI running in the browser with the power of a Python backend. This approach is ideal for those who are using Gemini, ChatGPT and Claude to create digital tools/small-apps that need to run locally.

- All the beauty of HTML, CSS and JS
- All the power of Python and its packages
- All running locally
- Easy to modify by vibe coding because all the code is on one page

This approach is also useful for privacy-sensitive applications or for demonstrations where a full-scale web deployment isn't necessary.<br>
Example: A doctor wants to create an app that uses an Ollama model to extract patient data from written records. For privacy reasons this app would need to run locally. Also, it would need a UI to ingest the written records and a Python backend to pre-process the data and to run the model. 

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

## How to run this code
In progress...

