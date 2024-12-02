<script>
    import axios from "axios";
    let userMessage = "";
    let chatHistory = [];
  
     //Basic predefined responses
     const responses = {
       "hello": "Hi there! How can I assist you today?",
       "how are you": "I'm just a bot, but I'm here to help!",
       "bye": "Goodbye! Have a great day!",
       "default": "I'm not sure about that. Can you rephrase?",

     };
  
     function handleSendMessage() {
       if (userMessage.trim() === "") return;
  
       // Add user message to chat history
       chatHistory = [...chatHistory, { sender: "user", message: userMessage }];
     //   // Add bot response to chat history
     const botResponse = responses[userMessage.toLowerCase()] || responses.default;
     chatHistory = [...chatHistory, { sender: "bot", message: botResponse }];
  
     // Clear input
     userMessage = "";
   }
  </script>
  
  <div class="chatbot-container">
    <div class="chat-window">
      {#each chatHistory as chat}
        <div class={`chat-bubble ${chat.sender}`}>
          {chat.message}
        </div>
      {/each}
    </div>
    <div class="chat-input">
      <input
        type="text"
        bind:value={userMessage}
        placeholder="Type a message..."
        on:keypress={(e) => e.key === 'Enter' && handleSendMessage()}
      />
      <button on:click={handleSendMessage}>Send</button>
    </div>
  </div>
  
  <style>
    .chatbot-container {
      display: flex;
      flex-direction: column;
      width: 300px;
      height: 400px;
      border: 1px solid #ccc;
      border-radius: 8px;
      overflow: hidden;
    }
    .chat-window {
      flex: 1;
      padding: 10px;
      overflow-y: auto;
      background-color: #f9f9f9;
    }
    .chat-bubble {
      margin: 5px 0;
      padding: 10px;
      border-radius: 10px;
      max-width: 70%;
    }
    .chat-bubble.user {
      background-color: #d1e7fd;
      align-self: flex-end;
    }
    .chat-bubble.bot {
      background-color: #f1f1f1;
      align-self: flex-start;
    }
    .chat-input {
      display: flex;
      border-top: 1px solid #ccc;
    }
    .chat-input input {
      flex: 1;
      padding: 10px;
      border: none;
      outline: none;
    }
    .chat-input button {
      padding: 10px 20px;
      border: none;
      background-color: #007bff;
      color: #fff;
      cursor: pointer;
      border-radius: 0;
    }
  </style>
  