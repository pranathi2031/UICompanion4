<script>
    export let showModal;
    export let closeModal;
    export let username; // This will be passed from the parent component
  
    let localUsername = username; // To use in the form input
    export let isSignedIn;
    function handleSubmit(event) {
      event.preventDefault();
      if (localUsername.trim() !== "") {
        // Set the prop value in the parent
        username = localUsername;
        isSignedIn = true;
        closeModal();
      }
    }
  </script>
  
  {#if showModal}
    <div class="modal-overlay" on:click={closeModal}>
      <div class="modal-content" on:click|stopPropagation>
        <button class="close-button" on:click={closeModal}>&times;</button>
        <h2>Sign In</h2>
        <form on:submit={handleSubmit}>
          <label for="username">Username:</label>
          <input
            type="text"
            id="username"
            bind:value={localUsername}  
            placeholder="Enter your username"
            required
          />
          
          <label for="password">Password:</label>
          <input
            type="password"
            id="password"
            placeholder="Enter your password"
            required
          />
          
          <button type="submit">Sign In</button>
        </form>
      </div>
    </div>
  {/if}
  
  <style>
    .modal-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .modal-content {
      background-color: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      width: 400px;
      position: relative;
    }
    .close-button {
      position: absolute;
      top: 10px;
      right: 10px;
      background: none;
      border: none;
      font-size: 24px;
      cursor: pointer;
    }
    form {
      display: flex;
      flex-direction: column;
    }
    label {
      margin-top: 10px;
    }
    input {
      padding: 8px;
      margin-top: 5px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    button {
      margin-top: 15px;
      padding: 10px;
      border: none;
      background-color:black;
      color: white;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover{
        background-color: rgb(69, 63, 63);
    }
  </style>
