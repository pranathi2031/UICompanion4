<script>
  let selectedMood = null;
  let timeAvailable = null;
  let locationType = null;

  const moods = ['Happy', 'Sad', 'Energetic', 'Lonely'];
  const times = ['<15 mins', '15-30 mins', '>30 mins'];
  const locations = ['Home', 'Outdoors', 'Office'];

  const recommendations = {
    Happy: {
      home: {
        short: ['Play a quick game', 'Sing along to your favorite song'],
        medium: ['Bake something fun', 'Call a friend'],
        long: ['Watch a feel-good movie', 'Host a virtual party'],
      },
      outdoors: {
        short: ['Take a selfie in the sun', 'Pick some flowers'],
        medium: ['Go for a nature walk', 'Visit a café'],
        long: ['Plan a picnic', 'Explore a new park'],
      },
      office: {
        short: ['Tell a joke to a colleague', 'Do some desk stretches'],
        medium: ['Organize your workspace', 'Plan your weekend'],
        long: ['Host a team bonding session', 'Start a creative project'],
      },
    },
    // Similar objects for 'Sad', 'Energetic', 'Lonely'
  };

  let filteredRecommendations = [];

  const generateRecommendations = () => {
    if (selectedMood && timeAvailable && locationType) {
      const moodRecs = recommendations[selectedMood.toLowerCase()] || {};
      const locationRecs = moodRecs[locationType.toLowerCase()] || {};
      const timeKey = timeAvailable.includes('<15') ? 'short' : timeAvailable.includes('15-30') ? 'medium' : 'long';
      filteredRecommendations = locationRecs[timeKey] || [];
    } else {
      filteredRecommendations = [];
    }
  };
</script>

<style>
  /* Add styles for inputs and recommendation cards */
</style>

<h1>Get Recommendations</h1>

<!-- Mood Selector -->
<div>
  <h3>Select your mood:</h3>
  {#each moods as mood}
    <button on:click={() => { selectedMood = mood; generateRecommendations(); }}>
      {mood}
    </button>
  {/each}
</div>

<!-- Time Availability Selector -->
<div>
  <h3>How much time do you have?</h3>
  {#each times as time}
    <button on:click={() => { timeAvailable = time; generateRecommendations(); }}>
      {time}
    </button>
  {/each}
</div>

<!-- Location Selector -->
<div>
  <h3>Where are you?</h3>
  {#each locations as location}
    <button on:click={() => { locationType = location; generateRecommendations(); }}>
      {location}
    </button>
  {/each}
</div>

<!-- Recommendations -->
{#if filteredRecommendations.length > 0}
  <div class="recommendation-card">
    <h2>Recommended Activities:</h2>
    <ul>
      {#each filteredRecommendations as recommendation}
        <li>{recommendation}</li>
      {/each}
    </ul>
  </div>
{:else if selectedMood || timeAvailable || locationType}
  <p>Please select all options to get recommendations.</p>
{/if}
