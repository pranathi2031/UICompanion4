<script>
  import Chatbot from "./Chatbot.svelte";
import Counter from "./lib/Counter.svelte";

  let go = false;
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
function goClick(){
  go = true;

}
function goUnclick(){
  go = false;
}

</script>
<main>
<nav class="navbar">
 <p class="nav-options"><i class="fas fa-heart"></i>
  Companion</p>
  <div class="nav-right">
  <button class="nav-button">Signin</button>
  <button class="nav-button">Register</button>
  <button class="nav-button"><i style="color:white;"class="fas fa-comments"></i></button>

  </div>
  </nav>
{#if go=== false}
<div class="screen1">
  <h1><i>Are you bored?

  </i>&#129300;
</h1>
  <h2><i>Let’s find something to lift your spirits</i></h2>
  <button class="goButton" on:click={goClick}>Lets Go</button>
</div>
{/if}
{#if go === true}
<div class="screen2">
  <button class="nav-buttons" on:click={goUnclick}><i class="fa-solid fa-arrow-left"></i></button>
  <h2>  Whats your mood?</h2>
  <div class="button-list">
    {#each moods as mood (mood.label)}
      <button class="mood-button" on:click={() => handleClick(mood)}>
        <i class={mood.icon} style="color:{mood.color}"></i>
        <br>
      </button>
    {/each}
  </div>
</div>
{/if}
<div class="chatbot-icon">
<i class="fas fa-comments"></i> <!-- Font Awesome chat icon -->
</div>

<Chatbot/>
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
.screen2{
left:55%;
position:relative;
}
.nav-options{
font-size: 20px;
font-weight: bold;
}

.button-list {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.mood-button {
  
  border: none;
  background-color: #94d2bd;
  padding: 10px 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 50px;
  
}

.button:hover {
  color: #78aa99;
}

.button i {
  margin-right: 8px;
}


.goButton{
  border-radius: 10px;
  font-size: larger;
  background-color:#ee9c04;
  color:white;
  border-color: #ee9c04;
  cursor: pointer;
}
.goButton:hover{
  background-color:#ca8506;
}
/* Styling the navigation bar */
.navbar {
display: flex;
align-items: center;

background-color:white; /* Dark background color */
padding: 10px 20px;
color:black; /* Text color */
box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
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
background-color: grey; /* Darker shade */
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
justify-content: right;
}





</style>