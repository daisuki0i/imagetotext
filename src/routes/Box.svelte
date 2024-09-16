<script>
    export let imageUrl = 'https://www.khaosod.co.th/wpapp/uploads/2024/09/Nong-Moo-Deng4548-5.jpg';
    export let description = 'HAHAHOHHOHO';
    function reconvert() {
        console.log("Reconvert was clicked.");
        // รอเขียนว่าจะให้ทำอะไร Reconvert
    }

    function clear() {
        console.log("Clear was clicked.");
        // รอเขียนว่าจะให้ทำอะไร Clear
    }
  </script>
  
  <style>
    .info-box {
      display: flex;
      align-items: center;
      background-color: #e0e0e0; 
      border-radius: 10px;
      padding: 60px;
      max-width: 600px;
      margin: 20px auto;      
    }
  
    .description {
      flex-grow: 1;
      margin-left: 10px;
    }
  
    img {
      width: 90px; 
      border-radius: 5px;
      /* margin-left: 10px; */
    }
  </style>
  
  <div class="info-box">
    <img src={imageUrl} alt="Image description"> 
    <div class="description">{description}
      <div class="buttons">
          <button on:click={reconvert}>Reconvert</button>
          <button on:click={clear}>Clear</button>
      </div>
  </div>
  </div>