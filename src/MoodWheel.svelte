<script>
    let selectedMood = '';
    let moodOptions = [
      { emoji: '😊', label: 'Happy', color: '#FFD700' },
      { emoji: '😢', label: 'Sad', color: '#1C1C1C' },
      { emoji: '😟', label: 'Anxious', color: '#FF6347' },
      { emoji: '😌', label: 'Calm', color: '#32CD32' }
    ];
  </script>
  
  <div style="display: flex; gap: 1rem;">
    {#each moodOptions as mood}
      <div
        on:click={() => (selectedMood = mood.label)}
        style="
          padding: 1rem;
          border: 2px solid #ccc;
          border-radius: 8px;
          background-color: {mood.color};
          text-align: center;
          cursor: pointer;
        "
      >
        <div style="font-size: 2rem;">{mood.emoji}</div>
        <p>{mood.label}</p>
      </div>
    {/each}
  </div>
  
  {#if selectedMood}
    <p>Your mood is: {selectedMood}</p>
  {/if}
  