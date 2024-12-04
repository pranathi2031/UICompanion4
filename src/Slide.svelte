<script>
    let scrollContainer;
  
    function scrollLeft() {
      scrollContainer.scrollBy({
        left: -740, // Adjust the value to change the scroll distance
        behavior: 'smooth'
      });
    }
  
    function scrollRight() {
      scrollContainer.scrollBy({
        left: 740, // Adjust the value to change the scroll distance
        behavior: 'smooth'
      });
    }
  </script>
  
  <style>
    .scroll-container {
      display: flex;
      align-items: center;
      overflow: hidden;
      width: 100%; /* Adjust as needed */
      position: relative;
      overflow-x: hidden;
    }
  
    .scroll-content {
      display: flex;
      scroll-behavior: smooth;
      overflow-x: scroll;
      width: 100%; /* Adjust as needed */
      white-space: nowrap;
      margin-left:30px;
      margin-right:30px;
    }
    .scroll-content::-webkit-scrollbar {
  display: none; /* Chrome, Safari, and Edge */
}
    .item {
      flex: 0 0 auto; /* Ensures items don't shrink */
      width: 500px; /* Adjust as needed */
      margin: 0 10px;
      background-color: lightgray;
      text-align: center;
      padding: 20px;
      
    }
  
    .scroll-left, .scroll-right {
      background:none;
      border: none;
      padding: 10px;
      cursor: pointer;
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      border-radius: 20px;

    }
  
    .scroll-left {
      left: 0;
    }
  
    .scroll-right {
      right: 0;
    }

    .card {
  min-width: 500px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s;
  margin-left:10px;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.card-image {
  width: 100%;
  height: 300px;
  object-fit: cover;
}

.card-heading {
  margin: 10px 0;
  font-size: 1.4rem;
  color: #333;
}

.card-text {
  margin: 0 10px 10px;
  color: #555;
  font-size: 1rem;
}
  </style>
  
  <div class="scroll-container">
    <button class="scroll-left" on:click={scrollLeft}>&#9664;</button>
    <div class="scroll-content" bind:this={scrollContainer}>
      <!-- Add your content here -->
      <div class="card">
        <img src="https://positive.b-cdn.net/wp-content/uploads/2022/11/Benefits-of-positive-emotions-300x150.jpeg" alt="Description of image 1" class="card-image" />
        <h2 class="card-heading">What Influences Your Mood?—and How to Improve Your Mood</h2>
        <p class="card-text">By Kendra Cherry, MSEd 
    
            </p>
      </div>
      <div class="card">
        <img src="path/to/image2.jpg" alt="Description of image 2" class="card-image" />
        <h2 class="card-heading">Article 2 Heading</h2>
        <p class="card-text">Brief description or content of the article goes here.</p>
      </div>
      <div class="card">
        <img src="path/to/image3.jpg" alt="Description of image 3" class="card-image" />
        <h2 class="card-heading">Article 3 Heading</h2>
        <p class="card-text">Brief description or content of the article goes here.</p>
      </div>
      <div class="card">
        <img src="path/to/image3.jpg" alt="Description of image 3" class="card-image" />
        <h2 class="card-heading">Article 3 Heading</h2>
        <p class="card-text">Brief description or content of the article goes here.</p>
      </div>
      <div class="card">
        <img src="path/to/image3.jpg" alt="Description of image 3" class="card-image" />
        <h2 class="card-heading">Article 3 Heading</h2>
        <p class="card-text">Brief description or content of the article goes here.</p>
      </div>
      <div class="card">
        <img src="path/to/image3.jpg" alt="Description of image 3" class="card-image" />
        <h2 class="card-heading">Article 3 Heading</h2>
        <p class="card-text">Brief description or content of the article goes here.</p>
      </div>
      
      <!-- Add more cards as needed -->
    </div>
    <button class="scroll-right" on:click={scrollRight}>&#9654;</button>
  </div>
  