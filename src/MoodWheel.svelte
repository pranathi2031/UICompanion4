<script>
  // Define the available moods and a variable for the selected mood.
  let moods = ['Happy', 'Sad', 'Energetic', 'Motivated', 'Amused', 'Fear', 'Lonely', 'Thrilled'];
  let selectedMood = null;

  // Function to handle the mood selection.
  function selectMood(mood) {
    selectedMood = mood;
  }
</script>

<style>
  .mood-board {
    display: flex;
    gap: 10px;
    justify-content: center;
    margin: 20px 0;
  }
  .mood {
    padding: 10px 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  .mood:hover {
    background-color: #f0f0f0;
  }
  .selected {
    background-color: #0088cc;
    color: white;
  }
</style>

<div class="mood-board">
  {#each moods as mood (mood)}
    <div
      class="mood {selectedMood === mood ? 'selected' : ''}"
      on:click={() => selectMood(mood)}
    >
      {mood}
    </div>
  {/each}
</div>

{#if selectedMood}
  <p>Your selected mood is: <strong>{selectedMood}</strong></p>
{/if}
