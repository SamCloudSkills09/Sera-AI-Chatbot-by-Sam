npx create-react-app sera-chatbot
cd sera-chatbot
npm install axios
// src/Chatbot.js
import React, { useState } from 'react';
import axios from 'axios';

const Chatbot = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');

  const handleSend = async () => {
    if (!input) return;

    const userMessage = { sender: 'user', text: input };
    setMessages([...messages, userMessage]);

    try {
      const response = await axios.post('https://api.openai.com/v1/engines/davinci-codex/completions', {
        prompt: input,
        max_tokens: 150,
      }, {
        headers: {
          'Authorization': `Bearer YOUR_API_KEY`,
          'Content-Type': 'application/json',
        },
      });

      const botMessage = { sender: 'sera', text: response.data.choices[0].text.trim() };
      setMessages([...messages, userMessage, botMessage]);
    } catch (error) {
      console.error('Error fetching response:', error);
    }

    setInput('');
  };

  return (
    <div className="chatbot">
      <div className="messages">
        {messages.map((msg, index) => (
          <div key={index} className={`message ${msg.sender}`}>
            {msg.text}
          </div>
        ))}
      </div>
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Type your message..."
      />
      <button onClick={handleSend}>Send</button>
    </div>
  );
};

export default Chatbot;
/* src/App.css */
.chatbot {
  width: 400px;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
}

.messages {
  height: 300px;
  overflow-y: scroll;
  margin-bottom: 10px;
}

.message {
  padding: 5px;
  margin-bottom: 5px;
  border-radius: 5px;
}

.message.user {
  background-color: #e1ffc7;
  text-align: right;
}

.message.sera {
  background-color: #f1f1f1;
  text-align: left;
}

input[type="text"] {
  width: calc(100% - 60px);
  padding: 5px;
}

button {
  width: 50px;
  padding: 5px;
}
// src/App.js
import React from 'react';
import './App.css';
import Chatbot from './Chatbot';

function App() {
  return (
    <div className="App">
      <h1>Chat with Sera</h1>
      <Chatbot />
    </div>
  );
}

export default App;
