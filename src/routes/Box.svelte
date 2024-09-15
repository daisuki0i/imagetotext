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
    function something() {
        console.log("Clear was clicked.");
        // รอเขียนว่าจะให้ทำอะไร Clear
    }
  </script>
  
  <style>
    .info-box {      
      align-items: center;
      background-color: #e0e0e0; 
      border-radius: 10px;
      padding: 20px;
      max-width: 600px;
      margin: 10px auto;     
      display: grid;
      grid-template-rows: 100px 50px ;
      grid-template-columns: 230px 230px;
      gap: 8px; 
    }
  
    .description {      
      align-items: center;  
      font-size: 10px;    
      flex-grow: 1;   
      margin-left: 10px;
      padding: 6px;
    }
      
    .description-box {      
      width: 200px;
      height: 100px;
      background-color: rgb(255, 255, 255);      
      justify-content: center;
      align-items: center;
    }        
    img {      
      width: 180px; 
      /* height: 90px; */
      border-radius: 5px;      
    }
  </style>
  
  <div class="info-box">   
    <img src={imageUrl} alt="Image description"> 
    <div class="description description-box">{description}                
      </div>
    <div>    
      <button on:click={something}>something</button>          
      </div>    
    <div class="buttons">      
          <button on:click={reconvert}>Reconvert</button>
          <button on:click={clear}>Clear</button>         
      </div>    
  </div>  
  