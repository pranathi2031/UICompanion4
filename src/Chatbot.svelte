<script>
  import axios from "axios";
  let userMessage = "";
  let chatHistory = [
    { sender: "bot", message: "Hi there! How can I assist you today?" }
  ]; // Default bot message
  export let showChatBot;
  
  // State to show the connecting screen
  let isConnecting = false;

  // Basic predefined responses
  const responses = {
    "hello": "Hi there! How can I assist you today?",
    "how are you": "I'm just a bot, but I'm here to help!",
    "bye": "Goodbye! Have a great day!",
    "i want to speak with someone": "Sure! please wait till I connect you with someone!!",
    "default": "I'm not sure about that. Can you rephrase?",
  };

  function handleSendMessage() {
    if (userMessage.trim() === "") return;

    // Add user message to chat history
    chatHistory = [...chatHistory, { sender: "user", message: userMessage }];

    // Check if the user message is "I want to speak with someone"
    if (userMessage.toLowerCase() === "i want to speak with someone") {
      // Set connecting state to true
      isConnecting = true;

      // Simulate a delay for connecting (e.g., 3 seconds)
      setTimeout(() => {
        // Add bot response to chat history after the delay
        chatHistory = [
          ...chatHistory,
          { sender: "bot", message: "Connecting you now, please wait..." }
        ];
        isConnecting = false; // Reset connecting state

        // Add the response after the "connecting" phase
        chatHistory = [
          ...chatHistory,
          { sender: "bot", message: "You are now connected to a representative." }
        ];
      }, 3000); // Adjust the delay as needed
    } else {
      // Add immediate response for other messages
      const botResponse = responses[userMessage.toLowerCase()] || responses.default;
      chatHistory = [...chatHistory, { sender: "bot", message: botResponse }];
    }

    // Clear input
    userMessage = "";
  }
</script>

<div class="chatbot-container">
  <div class="chat-window">
    <button class="close-button" on:click={() => (showChatBot = false)}>
      <i class="fas fa-close"></i>
    </button>
    {#each chatHistory as chat}
      <div class={`chat-bubble ${chat.sender}`}>
        {chat.message}
      </div>
    {/each}

    {#if isConnecting}
      <!-- Show loading message when connecting -->
      <div class="connecting-screen">
        <p>Connecting...</p>
        <div class="spinner"></div>
      </div>
    {/if}
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
    position: fixed;
    right: 10px;
    bottom: 10px;
  }
  .chat-window {
    flex: 1;
    padding: 10px;
    overflow-y: auto;
    background-color: #f9f9f9;
    position: relative;
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
    background-color: black;
    color: #fff;
    cursor: pointer;
    border-radius: 0;
    font-weight: bold;
  }
  .chat-input button:hover{
   background-color:  rgb(69, 63, 63);
  }
  .close-button {
    background-color: black;
    color: white;
    border-radius: 5px;
    font-weight: bold;
    font-size: 15px;
  }
  .close-button:hover{
    background-color:  rgb(69, 63, 63);;
  }
  .connecting-screen {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: rgba(255, 255, 255, 0.8);
    z-index: 1;
  }
  .spinner {
    margin-top: 10px;
    width: 24px;
    height: 24px;
    border: 4px solid #ccc;
    border-top: 4px solid #555;
    border-radius: 50%;
    animation: spin 1s linear infinite;
  }
  @keyframes spin {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }
  
</style>
