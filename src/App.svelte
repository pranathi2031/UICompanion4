<script>
  import Chatbot from "./Chatbot.svelte";
  import Counter from "./lib/Counter.svelte";
  import MoodWheel from "./MoodWheel.svelte";
  import image from "./lib/chatbot_01_16_bot_chat_robot_app_artificial_chatbot_dialog-1024.webp";
  import Signin from "./Signin.svelte";
  let username = "";
  let go = false;
  let showMessage= true;
  let message = "Welcome! How can I help you?";
  let showChatBot = false;
  let showModal = false;
  let isSignedIn = false;

  function openModal() {
    showModal = true;
  }

  function closeModal() {
    showModal = false;
  }
  
  function chatBot(){
    showChatBot = !showChatBot;
  }
  // Start displaying the message when the component loads
  import { onMount } from 'svelte';
  onMount(() => {
    setTimeout(() => {
      showMessage = true;
    }, 200000000000); // Delay in milliseconds (500ms here for a slight delay)
  });
// Toggle the menu on smaller screens


let moods = [
  { icon: 'fas fa-smile', label: 'Happy', color:'yellow' },
  { icon: 'fas fa-sad-tear', label: 'Sad', color:'grey'},
  { icon: 'fas fa-angry', label: 'Angry',color:'red' },
  { icon: 'fas fa-meh', label: 'Meh' ,color:'black'},
  { icon: 'fas fa-grin', label: 'Excited',color:'blue' },
];


function handleClick(mood) {
  alert(`You selected: ${mood.label}`);
}
</script>
<main>
<nav class="navbar">
 <p class="nav-options"><i class="fas fa-heart"></i>
  Companion</p>
  
  {#if isSignedIn}
  <div class="nav-right1">
  <button class="nav-button"on:click={() => (isSignedIn = false, username="")}>SignOut</button>
  <button class="nav-button"><i style="color:white;"class="fas fa-comments"></i></button>
  </div>
  {:else}
  <div class="nav-right">
  <button class="nav-button" on:click={openModal}>Signin</button>
  <button class="nav-button">Register</button>
  <button class="nav-button"><i style="color:white;"class="fas fa-comments"></i></button>
  </div>
  {/if}
  
  </nav>

<div class="screen1">
  <h1><i>{username} Are you bored?

  </i>
</h1>
  <h2><i>Let’s find something to lift your spirits</i></h2>
  
</div>
{#if !showChatBot}
<button class="chatbot-button" on:click={chatBot}>

  <!-- Use the icon -->
  <img src={image} style="height:60px; position:fixed; bottom:10px; right:10px;"/>
  </button>
{#if showMessage}
  <span class="chatbot-message">{message}</span>
{/if}
{/if}

<MoodWheel/>
{#if showChatBot}
<Chatbot bind:showChatBot={showChatBot}/>
{/if}

{#if showModal}
<Signin showModal={showModal} closeModal={closeModal} bind:username bind:isSignedIn/>
{/if}


</main>
<style>
.screen1{
  position:relative;
  z-index:10000;
  text-align: center;
  
}
h1{
font-size: 50px;
text-align:justify;
text-align: center;
}

.nav-options{
font-size: 20px;
font-weight: bold;
}
/* Styling the navigation bar */
.navbar {
display: flex;
align-items: center;

background-color:white; /* Dark background color */
padding: 10px 20px;
color:black; /* Text color */
box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
height:50px;
}

/* Styling the navigation options text */
.nav-options {
font-size: 1.5rem;
font-weight: bold;
display: flex;
align-items: center;
}

/* Styling the buttons */
.nav-button {
background-color: black; /* Button color */
color: #fff; /* Button text color */
border: none;
border-radius: 5px;
padding: 8px 12px;
margin: 0 5px;
cursor: pointer;
font-size: 1rem;
transition: background-color 0.3s, transform 0.3s;
font-weight: bold;
}

/* Button hover effects */
.nav-button:hover {
background-color: rgb(69, 63, 63); /* Darker shade */
transform: scale(1.05); /* Slightly enlarge button */
}

/* Button active state */
.nav-button:active {
background-color:grey; /* Even darker shade */
transform: scale(1);
}

/* Styling the icons */
.nav-options i, .nav-button i {
margin-right: 5px; /* Space between icon and text */
color:black;
}

/* Responsive adjustments */
@media (max-width: 768px) {
.navbar {
  flex-direction: column;
  align-items: center;
}

.nav-button {
  margin: 5px 0;
}
}
.nav-right{
  display: flex;
  margin-left:70%;
  gap: 10px;
}
.nav-right1{
  display: flex;
  margin-left:78%;
  gap: 10px;


}
.chatbot-button {
  border-radius: 30px;
  position: fixed; /* Use fixed for a sticky element relative to the viewport */
  bottom: 10px;
  right: 10px;
  /* Optional styling for appearance */
  padding: 10px 20px;
  background-color:white;
  color: white;
  border: none;
  cursor: pointer;
  height: 60px;
  width:60px;

}
.chatbot-button:hover{
  background-color: rgb(192, 192, 192);
}
.chatbot-message {
    display: inline-block;
    background-color: #f8f9fa; /* Light background */
    padding: 10px 20px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    font-size: 1rem;
    color: #333;
    opacity: 0; /* Start invisible */
    transform: translateY(70px); /* Start slightly above */
    animation: fadeIn 1s forwards; /* Animation definition */
   position: fixed;
   bottom:20px;
   right:5%
  }

  @keyframes fadeIn {
    0% {
      opacity: 0;
      transform: translateY(-20px);
    }
    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }




</style>