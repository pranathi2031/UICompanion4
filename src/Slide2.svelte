<script>
    export let show = false;
    export let activity = {};
    export let goal = "";
    export let onClose;
    export let onAddGoal;
    let newGoal = "";
    
  
    const handleAddGoal = () => {
      if (newGoal.trim()) {
        onAddGoal(newGoal);
        newGoal = "";
      }
    };
  </script>
  
  {#if show}
    <div class="modal-overlay" on:click={onClose}>
      <div class="modal" on:click|stopPropagation>
        <h2>{activity.name}</h2>
        <p>Minimum time: {activity.time}</p>
        <h3>Set a Goal</h3>
        <input
          type="text"
          placeholder="Enter your goal"
          bind:value={newGoal}
        />
        <button class="btn" on:click={handleAddGoal}>Add Goal</button>
        <button class="btn btn-close" on:click={onClose}>Close</button>
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
      border-radius: 10px;
      max-width: 400px;
      width: 90%;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    .btn-close {
      background-color: red;
      color: white;
    }
  </style>
  