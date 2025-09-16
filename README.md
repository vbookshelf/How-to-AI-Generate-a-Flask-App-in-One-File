# One Page Flask Web App Template for Vibe Coding
A simple example that shows how to create a one-page desktop app the combines a nice UI running in the browser with the power of a Python backend. All running locally. When you are vibe coding, this example code can be given to the LLM as reference material.

<br>
<br>

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
