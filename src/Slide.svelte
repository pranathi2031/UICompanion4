<script>
  import { onMount } from "svelte";
    import App from "./App.svelte";

  // Data for mood options and activities
  const moods = ["Happy", "Sad", "Energetic", "Relaxed"];
  const activities = {
    Happy: {"Dancing":{}, "Playing Games":{}},
    Sad: {"Meditation":{}, "Listening to Music":{}},
    Energetic: {"Workout":{}, "Jogging":{}},
    Relaxed: {"Reading":{}, "Watching Movies":{}},
    Angry:{"Walk":{},}
  };

  // State management
  let selectedMood = "";
  let selectedActivity = "";
  let recommendations = [];
  let userRecommendations = [];
  let goal = "";
  let showGoalFeedback = false;
  let statusMessage = "";
  let value= "";
  let statusBarColor = "gray";
  let statusBarWidth ="";
  let selectedTimeSpan="";
  let selectedPlace="";
  const handleActivitySelect = () => {
    if (selectedMood && selectedActivity) {
      recommendations = [
        { text: `${selectedActivity} Tutorial`, link: "#", liked: false },
        { text: `${selectedActivity} Tips`, link: "#", liked: false }
      ];
    }
  };

  const toggleLike = (recommendation) => {
    recommendation.liked = !recommendation.liked;
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
  function addGoal() {
    if (goal.trim()) {
      statusMessage = `Goal: "${goal}" is in progress`;
      showGoalFeedback = false;
      statusBarColor = "orange";
      statusBarWidth="50%"; // Set the status bar color to 'in-progress'
    } else {
      showGoalFeedback = true;
    }
  }

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
    width: 1000px;
    z-index: 10;
    left:553px;
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

.select-box-container {
  position: relative;
  width: 200px;
}
option:focus{
background-color: #007bff;
}
.select-box-container::after {
  content: '▼'; /* Custom arrow */
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  pointer-events: none;
  font-size: 16px;
  color: #007bff;
}
</style>

<section id="mood-selection">
  <h2>Select Your Mood</h2>
  <div>
    {#each moods as mood}
      <button class={selectedMood===mood?"selected":"btn"} on:click={() => {
        selectedMood = mood;
        recommendations = [];
        selectedActivity = "";
        smoothScroll('#activity-selection');
        console.log(selectedMood===mood);
      }}>
        {mood}
      </button>
    {/each}
  </div>
</section>
<!-- <div class="select-box-container">
  <label>Select your availability: </label>
<select id="time-span" bind:value={selectedTimeSpan} class="select-box">
  <option value=""> Select...</option>
  <option value="<30Min"> Less than 30 Minutes</option>
  <option value="ThirtyMin">Thirty Minutes</option>
  <option value="OneHour">One Hour</option>
  <option value=">One Hour">More than one hour</option>
</select><br>
</div>

<div class="select-box-container">
  <label>Where are you currently</label>
<select id="place" bind:value={selectedPlace} class="select-box">
  <option value=""> Select...</option>
  <option value="Home"> Home</option>
  <option value="Office">Office</option>
  <option value="Gym">Gym</option>
  <option value="Travelling">Travelling</option>
</select>

</div> -->





<section id="activity-selection">
  <h2>Activities you can do when you are {selectedMood.toLowerCase()|| ""}</h2>

  {#if selectedMood}
    {#each activities[selectedMood] as activity}
      <button class={selectedActivity===activity?"selected":"btn"} on:click={() => (selectedActivity = activity) && handleActivitySelect()}>
        {activity}
      </button><br>
    {/each}
  {:else}
    <p>Please select a mood first.</p>
  {/if}
</section>


{#if selectedActivity}
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
    <!-- <div
      class="status-bar"
      style="background-color: {statusBarColor}; width:{statusBarWidth}">
    </div>-->
    <div class="status-bar-container"> 
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
{/if}