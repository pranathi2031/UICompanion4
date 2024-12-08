<script>
  import Chatbot from "./Chatbot.svelte";
  import { onMount } from 'svelte';
  import Slide from "./Slide.svelte";
  import vegaEmbed from 'vega-embed';
    import Footer from "./Footer.svelte";
  
  let showMessage= true;
  let showChatBot = false;
  let showModal = false;
  let flipped = false;

    // Toggle the flipped state
    function toggleFlip() {
        flipped = !flipped;
    }
  function openModal() {
    showModal = true;
  }

  function closeModal() {
    showModal = false;
  }
  
  function chatBot(){
    showChatBot = !showChatBot;
  }
  
  let mood = "happy";  // Default mood value

  // Event handler for receiving the changed value from the child
  function handleMoodChange(event) {
    mood = event.detail; // Event detail contains the updated value
  }

  
  // Start displaying the message when the component loads
  onMount(() => {
    setTimeout(() => {
      showMessage = true;
    }, 200000000000); 
  // Delay in milliseconds (500ms here for a slight delay)
  });

function handleClick(mood) {
  alert(`You selected: ${mood.label}`);
}
let dateValue='';
let day = new Date().getDate();
let months =["Jan","Feb","March","April","May","June","July","August","September","October","November","December"];
let month = months[new Date().getMonth()];
dateValue = day + " "+month;
let selectedMood='Happy';

 let isSidePanelVisible = false; // State to control side panel visibility
 const toggleSidePanel = () => {
     isSidePanelVisible = !isSidePanelVisible; // Toggle panel visibility
 };
//export let updateFeelings;


let week = 1;

</script>

<main>
<nav class="navbar">
 <p class="nav-options"><i style="color:white;" class="fas fa-heart"></i>
  Companion</p>
  <div class="nav-right1">
    <span class="date">{dateValue}</span>
    <button class="nav-button" on:click={chatBot}>
      <i class="fa-solid fa-comments" style="font-size:25px; color:white;"></i>
    </button>
    <button class="nav-button" on:click={toggleSidePanel}><i class="fa-solid fa-user" style="font-size: 25px; color:white" ></i>
    </button>
  </div>
</nav>

<div class="screen1">
  <img src="https://images.pexels.com/photos/443446/pexels-photo-443446.jpeg?cs=srgb&dl=daylight-forest-glossy-443446.jpg&fm=jpg" width="100%" height="500px" class="screen1-image"/>
   <p style="" class="screen1-text">How do you feel today?</p> 
</div> 

{#if showChatBot}
<Chatbot bind:showChatBot={showChatBot}/>
{/if}
<Slide bind:selectedMood={selectedMood}
isSidePanelVisible={isSidePanelVisible}
toggleVisible={toggleSidePanel}
 />

<Footer/>
</main>
<style>

.screen1{
  position:relative;
  z-index:1;
  text-align: center;
  margin-top:0;
  width:100%;
  height:500px;
  background-color:darkgray;
}
.screen1-image{
  z-index:1;
  opacity: 0.5;
}
.screen1-text{
  font-size:70px; 
  bottom:380px;
  position:relative; 
  color:white; 
  font-weight:bolder;

}
.pic-moods{
  
  position:relative;
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
background: linear-gradient(to right, hsl(210, 94%, 44%), hsl(184, 99%, 52%),hsl(210, 94%, 44%));  
padding: 10px 20px;
color:white; /* Text color */
box-shadow: 0 4px 8px rgba(238, 232, 232, 0.2);
height:50px;
position:sticky;
z-index:20000;
top:0;
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
 /* Button color */
background:none;
color: black; /* Button text color */
border: none;
border-radius: 5px;
cursor: pointer;
font-size: 1rem;
transition: background-color 0.3s, transform 0.3s;
font-weight: bold;
height:40px;
}

/* Button hover effects */
.nav-button:hover {
transform: scale(1.05); /* Slightly enlarge button */
background: none;
color:azure;
}

/* Button active state */
.nav-button:active {
color:grey; /* Even darker shade */
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
.nav-right1{

  display: flex;
  margin-left:74%;
  gap:15px;
  


}
.chatbot-button {
  border-radius: 30px;
  position: fixed; /* Use fixed for a sticky element relative to the viewport */
  bottom: 10px;
  right: 10px;
  /* Optional styling for appearance */
  padding: 10px 20px;
  background-color:#4A90E2;
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
    background-color: black; /* Light background */
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
   right:5%;
   color:white;
   font-weight: bold;
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
  .date {
  color: white;
  font-size: 20px;
  width:120px;
  font-weight: bold;
  margin-top:10px;
}




</style>