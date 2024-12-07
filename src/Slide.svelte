<script>
  import { onMount } from "svelte";
    import App from "./App.svelte";
    import Modal from "./Slide2.svelte";
    import Slide2 from "./Slide2.svelte";

  // Data for mood options and activities
  const moods = ["Happy", "Sad", "Energetic", "Relaxed"];
  

  export let activities = {
      Happy: [
        { name: "Go for a walk", img: "https://th.bing.com/th/id/OIP.ehXIV6y_-Xyu9CPQPrskAwHaEK?w=327&h=184&c=7&r=0&o=5&dpr=1.3&pid=1.7",time:"20 min",location1:true,location2:true,location3:false,location4:true},
        { name: "Watch a comedy", img: "comedy.jpg",time:"10min",location1:true,location2:true,location3:true,location4:true},
        { name: "Dance", img: "dance.jpg", time:"10min",location1:true,location2:false,location3:false,location4:false},
        { name: "Dance", img: "dance.jpg",time:"10min",location1:true,location2:false,location3:false,location4:false},
        { name: "Dance", img: "dance.jpg", time:"10min",location1:true,location2:false,location3:false,location4:false},
        { name: "Dance", img: "dance.jpg",time:"10min",location1:true,location2:false,location3:false,location4:false},
        { name: "Dance", img: "dance.jpg",time:"10min",location1:true,location2:false,location3:false,location4:false},
      ],
      Sad: [
        { name: "Journal", img: "journal.jpg", time:"10min",location1:true,location2:true,location3:true,location4:true},
        { name: "Listen to music", img: "music.jpg", time:"2min",location1:true,location2:true,location3:true,location4:true},
      ],
      Relaxed: [
        { name: "Meditate", img: "meditate.jpg",time:"20min",location1:true,location2:true,location3:true,location4:true },
        { name: "Read a book", img: "read.jpg",time:"10min",location1:true,location2:true,location3:true,location4:true},
      ],
      Energetic: [
        { name: "Workout", img: "workout.jpg", time:"60min",location1:true,location2:true,location3:false,location4:false},
        { name: "Go for a run", img: "run.jpg", time:"30min",location1:true,location2:true,location3:false,location4:false},
      ],
    };
  
  console.log(activities["Happy"].length);
  // State management
  let selectedMood = "Happy";
  let selectedActivity = "";
  let recommendations = [];
  let userRecommendations = [];
  let showGoalFeedback = false;
  let statusMessage = "";
  let value= "";
  let statusBarColor = "gray";
  let statusBarWidth ="";
  
  let currentIndex =0;
  const entries = 3; // Number of cards to show per view
  let isDisabledPrev = true;
  let isDisabledNext = false;
const handleActivitySelect = () => {
    if (selectedMood && selectedActivity) {
      recommendations = [
        { text: `${selectedActivity} Tutorial`, link: "#", liked: false },
        { text: `${selectedActivity} Tips`, link: "#", liked: false }
      ];
    }
  };

  
  const addUserRecommendation = () => {
    if (value) {
      userRecommendations = [...userRecommendations, value];
    }
    value="";
    console.log(userRecommendations);
  };

  // const addGoal = () => {
  //   if (!goal) {
  //     showGoalFeedback = true;
  //   } else {
  //     showGoalFeedback = false;
  //     statusMessage = "Goal added. Status: In Progress";
  //   }
  // };
  // function addGoal() {
  //   if (goal.trim()) {
  //     statusMessage = `Goal: "${goal}" is in progress`;
  //     showGoalFeedback = false;
  //     statusBarColor = "orange";
  //     statusBarWidth="50%"; // Set the status bar color to 'in-progress'
  //   } else {
  //     showGoalFeedback = true;
  //   }
  // }

  const markCompleted = (g) => {
    statusMessage = "🎉" +g+" Goal Completed! Well done! 🎉";
    goal="";
    statusBarColor="green";
    statusBarWidth="100%";
  };

  function smoothScroll(target) {
  const element = document.querySelector(target);
  element.scrollIntoView({ behavior: 'smooth' });
}
function selectMood(mood) {
    selectedMood = mood;
    currentIndex = 0;
    updateButtonStates();
  }

  function getNextRec() {
    const totalActivities = activities[selectedMood]?.length || 0;
    if (currentIndex + entries < totalActivities) {
      currentIndex += entries;
    }
    updateButtonStates();
  }

  function getPrevRec() {
    if (currentIndex > 0) {
      currentIndex -= entries;
    }
    updateButtonStates();
  }

  function updateButtonStates() {
    const totalActivities = activities[selectedMood]?.length || 0;
    isDisabledPrev = currentIndex <= 0;
    isDisabledNext = currentIndex + entries >= totalActivities;
  }

  
  

  let showModal = false;
  
  
  

  let selectedActivity1 = {};
  let goal = "";

  const openModal = (activity) => {
    selectedActivity1 = activity;
    console.log(selectedActivity1);
    showModal = true;
  };

  const closeModal = () => {
    showModal = false;
    console.log(showModal);
  };

  const addGoal = (newGoal) => {
    goal = `Goal: ${newGoal}`;
    closeModal();
  };
  let liked = false;  // Variable to track like/unlike state

  // Function to handle button click
  function toggleLike() {
    liked = !liked;
    
    // Record the interaction (e.g., log it or send it to a server)
    if (liked) {
      console.log('User liked the post');
      // You could also store this information in local storage or send it to a backend
    } else {
      console.log('User unliked the post');
      // Handle unliking the post as needed
    }
  }

 let filteredActivities=[];
 let selectedTimeSpan;
 let selectedPlace;
 function filterActivities(){
  filteredActivities=[];
  for (let i = 0; i < activities[selectedMood].length; i++) {
      const activity = activities[selectedMood][i];
      const timeMatch = checkTime(activity.time);
      const locationMatch = checkLocation(activity);

      if (timeMatch && locationMatch) {
        filteredActivities.push(activity);
      }
    }
    filteredActivities=filteredActivities;
 }
 console.log(filteredActivities);
  // Helper function to check if the activity time matches the selected time span
  const checkTime = (time) => {
    if (!selectedTimeSpan) return true; // If no time filter is selected, allow all activities
    const timeParts = time.split(' ');
    const timeValue = parseInt(timeParts[0]);
    console.log(timeValue);

    switch (selectedTimeSpan) {
      case "<30Min":
        return timeValue < 30;
      case ">30min<1hr":
        return (timeValue>=30 && timeValue<60);
      case ">1hr":
        return timeValue>=60;
      default:
        return true;
    }
  };
  
  const checkLocation = (location) => {
    if (!selectedPlace) return true; // If no time filter is selected, allow all activities
    
    switch (selectedPlace) {
      case "Home":
        return location.location1;
      case "Office":
        return location.location2;
      case "Vehicle":
        return location.location3;
      case "Travel Lounge":
        return location.location4;    
      default:
        return true;
    }
  };


  // Watch for changes in the selected mood, time span, or place and apply filters
  $: selectedMood, selectedTimeSpan, selectedPlace, filterActivities();
  </script>

<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    background: #f7f7f7;
  }
  section {
    padding: 1rem;
    margin-bottom: 2rem;
    background: #fff;
    
  }
  #mood-selection{
    margin-left:20px;
  }
  
  h1, h2, h3 {
    color: #333;
  }
  .btn {
    display: inline-block;
    margin-top: 1rem;
    padding: 1rem 1.5rem;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    background-color: white;
    color: black;
    border-color: #1e90ff;
    border-radius: 5px;
    cursor: pointer;
    text-align: center;
    margin-left: 20px;
    font-size: 15px;
    border-width: 3px;
  }
  .btn:hover {
    background-color:#4b5fc1;
  }
  .error {
    color: red;
    margin-top: 0.5rem;
  }
  .success {
    background: #d4edda;
    color: #155724;
    padding: 0.5rem;
    border-radius: 5px;
  }
  .status-bar {
    height: 20px;
    border-radius: 5px;
    margin-top: 15px;
    transition: background-color 0.5s ease;
    border-width: 100%;
  }

  .like {
    cursor: pointer;
    color: #ccc;
  }
  .liked {
    color: gold;
  }
  .selected{
    background-color:#1e90ff;
    display: inline-block;
    margin-top: 1rem;
    padding: 1rem 1.5rem;
    color: white;
    border-color: #1e90ff;
    border-radius: 5px;
    cursor: pointer;
    text-align: center;
    margin-left: 20px;
    font-size: 15px;
    border-width: 3px;
    font-weight: bolder;
    
  
  }
  .selected:hover{
    background-color: #1e90ff;
  }

  .status-bar-container {
    width: 100%;
    height: 30px;
    background-color: #e0e0e0; /* Light gray for the background */
    border-radius: 5px;
    overflow: hidden; /* Ensures no overflow beyond the container */
    margin: 20px 0;
  }

  .status-bar-fill {
    height: 100%;
    width: 0; /* Dynamically updated */
    background: linear-gradient(to right, #1e90ff, #4682b4); /* Blue gradient */
    transition: width 0.5s ease; /* Smooth animation for updates */
  }
  #activity-selection{
    transform: translateX(-50%);
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    width: 87%;
    z-index: 10;
    left:730px;
    bottom:40px;
    position:relative;
  }

  .select-box {
  padding: 10px;
  font-size: 16px;
  background-color: #f8f8f8;
  border: 1px solid #ccc;
  border-radius: 5px;
  appearance: none; /* Removes default arrow */
  width: 100%;
}

.select-box:focus {
  border-color: #007bff;
  outline: none;
}

/* Container styling for the select box */
.select-box-container {
  position: relative;
  width: 200px;
  margin-bottom: 10px;
  font-family: Arial, sans-serif;
}

/* Custom arrow for the dropdown */
/* Select box container with relative positioning */
.select-box-container {
  position: relative;
  width: 200px;
  margin-bottom: 10px;
  font-family: Arial, sans-serif;
}

/* Custom dropdown arrow */
.select-box-container::after {
  content: '▼'; /* Custom arrow */
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  pointer-events: none;
  font-size: 16px;
  color: #007bff;
  transition: transform 0.3s ease, color 0.3s ease;
}

/* Rotate arrow when select box or its children are focused */
.select-box-container:focus-within::after {
  transform: translateY(-50%) rotate(180deg); /* Rotate arrow upwards */
  color: #0056b3;
}

/* Style the select box */
select {
  width: 100%;
  padding: 10px;
  padding-right: 30px; /* Account for the arrow */
  border: 2px solid #007bff;
  border-radius: 5px;
  background: #ffffff;
  appearance: none; /* Hide default browser arrow */
  cursor: pointer;
  font-size: 14px;
  color: #333;
  outline: none;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

/* Hover and focus styles for the select box */
select:hover {
  border-color: #0056b3;
}

select:focus {
  border-color: #0056b3;
  box-shadow: 0 0 4px rgba(0, 123, 255, 0.5);
}

/* Native select box styling */

/* Styling for focused option */
option:focus {
  background-color: #007bff;
  color: #ffffff;
}
.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    
  }

  .card {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 350px;
    padding: 1rem;
    border-width: 3px;
    border-style: solid; /* Add this to define the border style */
    border-color: #007bff; /* This will now work */
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    color:black;
   /* Optional: Add a background color to make text readable */
}

  .card-icon img {
    width: 50px;
    height: 50px;
    margin-right: 1rem;
    border-radius: 5px;
    background-color: #f0f0f0;
  }

  .card-content {
    flex-grow: 1;
  }

  .card h3 {
    margin: 0;
    font-size: 1.2rem;
  }

  .card p {
    margin: 0.5rem 0;
    font-size: 0.9rem;
  }

  .card-action .btn {
    margin-top: 0.5rem;
    color: white;
    background-color:#007bff;
    font-size: 0.9rem;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .card-action .btn:hover {
    background-color: #fff4e1;
  }

  .recommendations {
    flex: 1;
    padding: 1rem;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  .recommendations h3 {
    margin-top: 0;
  }

  .recommendations ul {
    padding-left: 1.5rem;
  }

  .arrow-button {
    padding-left:3px;
    padding-right:3px;
    padding-top:1px;
    padding-bottom:1px;
    border: none;
    background:none;
    color:#007bff;
    cursor: pointer;
    border-radius: 5px;
  }
  .arrow-button:disabled {
    background:none;
    color:#ccc;
    cursor: not-allowed;
  }
  .liked {
    color: #ff4b4b;  /* Color when liked */
  }
  .unliked {
    color: #444;  /* Default color when not liked */
  }
 
</style>

<section id="mood-selection">
  <h2>Select Your Mood</h2>
  <div>
    {#each moods as mood}
      <button class={selectedMood===mood? "selected":"btn"} on:click={() => {
        selectedMood = mood;
        recommendations = [];
        selectedActivity = "";
        smoothScroll('#activity-selection');
        console.log(selectedMood===mood);
        selectMood(mood);
      }}>
        {mood}
      </button>
    {/each}
  </div>
</section>





<section id="activity-selection">
  <h2>Recommended Activities</h2>

  <div class="select-box-container">
    <label>Select your availability: </label>
  <select id="time-span" bind:value={selectedTimeSpan} on:change={filterActivities} class="select-box">
    <option value=""> Select...</option>
    <option value="<30Min"> Less than 30 Minutes</option>
    <option value=">30min<1hr">More than 30 min and less than an hour</option>
    <option value=">1hr">More than one hour</option>
  </select>
  </div>
  
  <div class="select-box-container1">
    <label>Where are you currently:</label>
  <select id="place" bind:value={selectedPlace} on:change={filterActivities} class="select-box">
    <option value=""> Select...</option>
    <option value="Home"> Home</option>
    <option value="Office">Office</option>
    <option value="Vehicle">Waiting in a Vehicle</option>
    <option value="Travel Lounge">Travel Lounge</option>
  </select>
  
  </div>
  

  {#if selectedMood}
    {#if filteredActivities.length>0}
    <div class="card-container">
      <!-- Loop through the activities for the selected mood -->
      {#if filteredActivities.length > entries}<button class="arrow-button" on:click={getPrevRec} disabled={isDisabledPrev}>◀</button>{/if}
      <div class="card-container">
        {#each filteredActivities?.slice(currentIndex, currentIndex + entries) as activity}
        <div class="card" on:click={() => openModal(activity)}>
          <div class="card-icon">
            <img src={activity.img} alt={activity} />
          </div>
          <div class="card-content">
            <button 
            class={liked ? 'liked' : 'unliked'} 
            on:click={toggleLike} style="z-index:1000">
            {liked ? 'Unlike' : 'Like'}
            
          </button>
            <h3>{activity.name}</h3>
            <p>Category: {selectedMood}</p>
            <p>Minimum time:{activity.time}</p>
            <p>{goal}</p>
            {#if goal}{/if}
          </div>
          <!-- <div class="card-action">
            <button
              class="btn"
              on:click={() => {
                selectedActivity = activity;
                handleActivitySelect();
              }}>
              Select
            </button>
          </div>-->
        </div> 
      
        {/each}
      </div>
      {#if filteredActivities.length>entries}<button class="arrow-button" on:click={getNextRec} disabled={isDisabledNext}>▶</button>{/if}
    
    </div>
    {:else}
    No activities based on your selected choices.
    {/if}
  {:else}
    <p>Please select a mood first.</p>
  {/if}
</section>

<Slide2
  show={showModal}
  activity={selectedActivity1}
  onClose={closeModal}
  onAddGoal={addGoal}
/>






<!-- {#if selectedActivity}
<section id="recommendations">
  <h2>Recommendations for {selectedActivity || "Activity"}</h2>
  {#if recommendations.length}
    <ul>
      {#each recommendations as recommendation}
        <li>
          <a href={recommendation.link} target="_blank">{recommendation.text}</a>
          <span
            class={recommendation.liked ? "liked" : "like"}
            on:click={() => toggleLike(recommendation)}>
            ★
          </span>
        </li>
      {/each}
    </ul>
  {/if}
</section>

<section id="add-recommendation">
  <h2>Add Your Own Recommendation</h2>
  <input
    type="text"
    placeholder="Add your recommendation"
    bind:value={value}
  />
  <button class="btn" on:click={addUserRecommendation}>Add Recommendation</button>
  <ul>
    {#each userRecommendations as rec}
      <li>{rec}</li>
    {/each}
  </ul>
</section>

<section id="goal-setting">
  <h2>Set a Goal for {selectedActivity || "Activity"}</h2>
  <input
    type="text"
    placeholder="Enter your goal"
    bind:value={goal} />
  <button class="btn" on:click={addGoal}>Add Goal</button>
  {#if showGoalFeedback}
    <p class="error">Please enter a goal before adding!</p>
  {/if}
  {#if statusMessage}
    <div class={statusMessage.includes("Completed") ? "success" : ""}>{statusMessage}</div>
    <div
      class="status-bar"
      style="background-color: {statusBarColor}; width:{statusBarWidth}">
    </div>-->
    <!-- <div class="status-bar-container"> 
      <div
        class="status-bar-fill"
        style=" background-color:{ statusBarColor} width: {statusBarWidth};">
      </div>
    </div>
    {#if !statusMessage.includes("Completed")}
      <button class="btn" on:click={() => markCompleted(goal)}>Mark as Completed</button>
    {/if}
  {/if}
</section>
{/if} --> 