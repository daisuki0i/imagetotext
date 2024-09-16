<script lang="ts">
  export let imageUrl: string;
  export let text: string = "Generated text will appear here...";
  export let language: string[] = []; // รับค่าเป็น array ของภาษาหลายภาษา
  export let clearResult: () => void; // รับฟังก์ชัน clearResult จาก +page.svelte

  // ฟังก์ชันสำหรับจัดการปุ่ม Clear
  function handleClear() {
    clearResult(); // เรียกใช้งาน clearResult ที่ถูกส่งเข้ามาจาก +page.svelte
  }

  // ฟังก์ชันสำหรับการดาวน์โหลด
  function downloadFile() {
    alert("Download button clicked.");
  }
</script>

<div class="result-container">
  <div class="image-container">
    <img src={imageUrl} alt="Result Image" class="result-image" />
  </div>
  <div class="right-container">
    <textarea class="result-text">{text}</textarea>
    <div class="button-group">
      <!-- แสดงภาษาที่ผู้ใช้เลือกทั้งหมดในรูปแบบที่อ่านง่าย -->
      <button class="language-button">
        {language.map((lang) => (lang === "Thai" ? "TH" : "EN")).join(" & ")}
      </button>
      <div class="action-buttons">
        <button class="clear-button" on:click={handleClear}>Clear</button>
        <button class="reconvert-button">Re-convert</button>
        <!-- ปุ่มดาวน์โหลด -->
        <button class="download-button" on:click={downloadFile}>Download</button>
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
    width: 100%;
    gap: 10px;
  }

.image-container {
  width: 120px; /* กำหนดขนาดความกว้างเท่ากับความสูง */
  height: 120px;
  overflow: hidden;
  border-radius: 10px;
  border: 1px solid #ccc;
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
  }

  .clear-button:hover,
  .reconvert-button:hover,
  .download-button:hover {
    background-color: #f2f2f2;
  }
</style>