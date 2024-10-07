<script lang="ts">
  import { onMount } from "svelte";
  import speakerIcon from "$lib/images/speaker.svg"

  export let imageUrl: string;
  export let text: string = "Generated text will appear here...";
  export let language: string[] = []; // รับค่าเป็น array ของภาษาหลายภาษา
  export let clearResult: () => void; // รับฟังก์ชัน clearResult จาก +page.svelte
  export let speech: string;

  let thai = "";
  let eng = "";

  // ฟังก์ชันสำหรับจัดการปุ่ม Clear
  function handleClear() {
    clearResult(); // เรียกใช้งาน clearResult ที่ถูกส่งเข้ามาจาก +page.svelte
  }

  // ฟังก์ชันสำหรับการดาวน์โหลด
  function downloadFile() {     
      // แยกชื่อไฟล์จาก URL ของภาพ
      const imageName = imageUrl.split("/").pop();

      const csvContent = `English,Thai\n${eng},${thai}\n`;

      const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' }); // สร้าง Blob จากข้อมูล CSV

      // สร้างลิงก์ดาวน์โหลด
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `${imageName}_caption.csv`; // ชื่อไฟล์ CSV ที่จะดาวน์โหลด
      a.click();
      
      URL.revokeObjectURL(url);// ลบลิงก์ที่ดาวน์โหลดเสร็จ
      alert("Downloading..."); 
  }

  onMount(() => {
    if (language.length === 1 && language[0] === "English") {
        eng = text;
      } else if (language.length === 1 && language[0] === "Thai") {
        thai = text;
      } else {
        [eng, thai] = text.split("\n");
      }
  });
</script>

<div class="result-container">
  <div class="image-container">
    <img src={imageUrl} alt="Result Image" class="result-image" />
    <!-- แสดงภาษาที่ผู้ใช้เลือกทั้งหมดในรูปแบบที่อ่านง่าย -->
    <button class="language-button">
      {#if language.length === 1}
        {language[0] === "English" ? "EN" : "TH"}
      {:else}
        EN & TH
      {/if}
    </button>
  </div>
  <div class="right-container">
    {#if eng}
      <textarea class="result-text" bind:value={eng}></textarea>
    {/if}
    {#if thai}
      <textarea class="result-text" bind:value={thai}></textarea>
    {/if}
    <div class="button-group">
      <div class="action-buttons">
        <button class="reconvert-button" on:click={() => {
          const audio = new Audio();
          audio.src = `data:audio/mp3;base64,${speech}`;
          audio.play();
        }}>
          <img src={speakerIcon} alt="Speaker Icon" width="14" height="14" />
        </button>
        <button class="clear-button" on:click={handleClear}>Clear</button>
        <!-- ปุ่มดาวน์โหลด -->
        <button class="download-button" on:click={downloadFile}>Download</button>

        <button class="download-button" on:click={() => {
          const text = `<img src="path/to/image" alt="${eng}/${thai}"></img>`;
          navigator.clipboard.writeText(text);
          alert("Copied to clipboard!");
        }}>Copy Code</button>
      </div>
    </div>
  </div>
</div>

<style>
  .result-container {
    border: 1px solid #ddd;
    border-radius: 12px;
    padding: 15px;
    display: flex;
    align-items: center;
    gap: 10px;
    background-color: #ffffff;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    max-width: 480px;
    width: 480px;
    margin: 10px auto;
    margin-top: 30px;
  }

  .right-container {
    display: flex;
    flex-direction: column;
    align-items: end;
    width: 100%;
    gap: 10px;
  }

.image-container {
  width: 120px; /* กำหนดขนาดความกว้างเท่ากับความสูง */
  height: 120px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
  /* overflow: hidden; */
  border-radius: 10px;
}

.result-image {
  width: 100%; /* กำหนดความกว้างให้เต็มกรอบ */
  height: 100%; /* กำหนดความสูงให้เต็มกรอบ */
  object-fit: cover; /* ทำให้ภาพถูกครอบให้อยู่ในกรอบที่กำหนด */
  border-radius: 10px; /* เพิ่มความมนของขอบ */
}


  .result-text {
    width: 95%;
    height: 70px;
    resize: none;
    padding: 8px;
    font-size: 14px;
    border: 1px solid #ddd;
    border-radius: 8px;
  }

  .button-group {
    display: flex;
    gap: 10px;
    align-items: center;
    justify-content: space-between;
  }

  .language-button {
    border: none;
    padding: 6px 12px;
    cursor: default;
    font-size: 14px;
    background-color: #e0e0e0;
    border-radius: 5px;
    text-transform: uppercase;
  }

  .action-buttons {
    display: flex;
    gap: 6px;
  }

  .clear-button,
  .reconvert-button,
  .download-button {
    padding: 6px 12px;
    border: 1px solid #000;
    background-color: #ffffff;
    cursor: pointer;
    border-radius: 6px;
    transition: background-color 0.3s;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .clear-button:hover,
  .reconvert-button:hover,
  .download-button:hover {
    background-color: #f2f2f2;
  }
</style>