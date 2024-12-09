<script>
  import { fly } from 'svelte/transition';
    export let show = false;
    export let activity = {};
    export let goal=[];
    export let onClose;
    export let onAddGoal;
    
    let newGoal = '';
    let goals = [];
    
    
  
  
  
    const handleAddGoal = () => {
      if (newGoal.trim()) {
        onAddGoal(newGoal);
        newGoal = "";
      }
    };
    let notification = '';

    // Predefined goal ideas
    let  goalIdeas = [
      {name:'Cook',goal:'Bake some cookies'},
      {name:'Cook',goal:'Make pasta'},
      {name:'Start a new hobby',goal:'Paint'},
      {name:'Start a new hobby',goal:'Click some pictures'},
      {name:'Do a fast dance',goal:'Dance for a bollywood song'},
      {name:'Do a fast dance',goal:'Dance for a mashup'}
      
      
      
      
      
      
    ];

    // Goal status constants
    const GOAL_STATUS = {
        NOT_STARTED: 'Not Started',
        IN_PROGRESS: 'In Progress',
        COMPLETED: 'Completed'
    };

    // Add new goal
    function addGoal() {
        const trimmedGoal = newGoal.trim();
        if (trimmedGoal === '') {
            alert("Goal cannot be empty!");
            return;
        }
        if (!goals.some(goal => goal.name === trimmedGoal)) {
            goals = [...goals, { name: trimmedGoal, status: GOAL_STATUS.NOT_STARTED,activity:activity.name }];
            newGoal = '';
        } else {
            alert("This goal already exists!");
            newGoal='';
        } 
        
        goal[activity.name]=goals.filter(goal=> goal.activity === activity.name).length; 
        console.log(goal);
    }
    function updateList(idea){
      const trimmedIdea = idea.trim();
      if (!goals.some(goal => goal.name === trimmedIdea)) {
            goals = [...goals, { name: trimmedIdea, status: GOAL_STATUS.NOT_STARTED,activity:activity.name }];
            
            newGoal = '';
        } else {
            alert("This goal already exists!");
            newGoal='';
        } 
        goal[activity.name]=goals.filter(goal=> goal.activity === activity.name).length; 
        
    
    }

    // Remove goal
    function removeGoal(index) {
        goals = goals.filter((_, i) => i !== index);
        goal[activity.name]=goals.filter(goal=> goal.activity === activity.name).length; 
        
     
    
    
    }

    // Mark goal as in progress
    function startProgress(index) {
        if (goals[index].status === GOAL_STATUS.NOT_STARTED) {
            goals[index].status = GOAL_STATUS.IN_PROGRESS;
            
        }
        goal[activity.name]=goals.filter(goal=> goal.activity === activity.name).length; 
        
    
    }

    // Mark goal as completed
    function completeGoal(index) {
        if (goals[index].status === GOAL_STATUS.IN_PROGRESS) {
            goals[index].status = GOAL_STATUS.COMPLETED;
            notification = `🎉 Goal "${goals[index].name}" completed!`;
            setTimeout(() => (notification = ''), 3000);
          
        }
        goal[activity.name]=goals.filter(goal=> goal.activity === activity.name).length; 
        
    
    
    }
    // Calculate counts
    $: totalGoals = goals.length;
    $: completedGoals = goals.filter(goal => goal.status === GOAL_STATUS.COMPLETED).length;
    $:inprogressGoals = goals.filter(goal => goal.status===GOAL_STATUS.IN_PROGRESS).length;
    
  </script>
  
  {#if show}
    <div class="modal-overlay" on:click={onClose}>
      <div class="modal" on:click|stopPropagation>
        <h2>{activity.name}</h2>
        <p style="font-weight:bold;">Minimum time: {activity.time}</p>
        <h3>Set a Goal</h3>
        <!-- <input
          type="text"
          placeholder="Enter your goal"
          bind:value={newGoal}
        />
        <button class="btn" on:click={handleAddGoal}>Add Goal</button> -->
        <input
        type="text"
        bind:value={newGoal}
        placeholder="Type your goal here"
        aria-label="Goal input"
        class="goalInput"
    />
    <button class="normalButton goal_button" on:click={addGoal}>Add Goal</button>

    <h3>Goal Ideas</h3>
    <ul class="goal-list">
        {#each goalIdeas as idea}
            {#if idea.name === activity.name}
            <li>
                <button class="normalButton"on:click={() => {updateList(idea.goal) }}>ADD "{idea.goal.toUpperCase()}"</button>
            </li><br>
            {/if}
        {/each}
    </ul>

    <h3>Your Goals</h3>
    <ul class="goal-list">
      {#if goals.length===0}
      <li style="color:red;" class="goal-item">You have no goals currently!</li>
      {:else}
        {#each goals as goal, index (goal.name)}
           {#if goal.activity===activity.name}
            <li class="goal-item {goal.status === GOAL_STATUS.COMPLETED ? 'completed' : (goal.status === GOAL_STATUS.IN_PROGRESS ? 'in-progress' : '')}" in:fly={{ y: 20, duration: 300 }}>
                
                {goal.name} ({goal.status})
                
                <div>
                    {#if goal.status === GOAL_STATUS.NOT_STARTED}
                        <button class="normalButton"on:click={() => startProgress(index)}>Start</button>
                    {/if}
                    {#if goal.status === GOAL_STATUS.IN_PROGRESS}
                        <button class="normalButton"on:click={() => completeGoal(index)}>Complete</button>
                    {/if}
                    <button class="normalButton"on:click={() => removeGoal(index)}>Remove</button>
                
                </div>
                
              </li>
            {/if}
        {/each}
        {/if}
    </ul>
    
   
       

    {#if notification}
        <div class="notification" in:fly={{ y: 100, duration: 400 }}>
            {notification}
        </div>
    {/if}
    <br>
        <button class="normalButton" on:click={onClose}>Close</button>
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
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 1000;
    }
    .modal {
      background: #fff;
      padding: 1.5rem;
      padding-top:1rem;
      border-radius: 10px;
      max-width: 400px;
      width: 90%;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    
    .goal-list {
     list-style-type: none;
     padding: 0;
 }
 .goal-item {
     display: flex;
     justify-content: space-between;
     margin: 5px 0;
     align-items: center;
     transition: background-color 0.2s;
     font-size:15px;
 }
 .goal-item.completed {
    
     color: green;
     background-color: #e0ffe0;
 }
 .goal-item.in-progress {
     color: orange;
 }
 .notification {
     margin: 10px 0;
     background-color: lightgreen;
     padding: 10px;
     border-radius: 5px;
     text-align: center;
 }
 .goal{
  align-content: center;
  margin-left: 30px;
  margin-right: 20px;
  min-height:600px;
  width:95%;
  font-size: 10px;
  color: black;
  background-color: white;
  border-radius: 50px;
  border-color: black;
  border-width: 30px;
  position: relative;
  padding: 10px;
  padding-bottom: 20px;

}
.goal_button{
    color:white;
    background-color:#1e90ff;
}
h2{
    text-align: center;
    font-size: 30px;
}
input{
  font-size: 15px;
}
h3{
font-size:20px;
}
.normalButton{
  background-color: #1e90ff;
  color:white;
  border-color: #1e90ff;
  border-radius: 5px;
  cursor: pointer;
  text-align: center;
  font-size: 15px;
  border-width: 3px;
  
}
</style>
  