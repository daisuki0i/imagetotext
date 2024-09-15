<script lang="ts">
    export let imageUrl: string;
    export let text: string = "Generated text will appear here...";
    export let language: string[] = []; // รับค่าเป็น array ของภาษาหลายภาษา
    export let clearResult: () => void; // รับฟังก์ชัน clearResult จาก +page.svelte
  
    // ฟังก์ชันสำหรับจัดการปุ่ม Clear
    function handleClear() {
      clearResult(); // เรียกใช้งาน clearResult ที่ถูกส่งเข้ามาจาก +page.svelte
    }
  </script>
  
  <div class="result-container">
    <div class="image-container">
      <img src={imageUrl} alt="Result Image" class="result-image" />
    </div>
    <textarea class="result-text" readonly>{text}</textarea>
    <div class="button-group">
      <!-- แสดงภาษาที่ผู้ใช้เลือกทั้งหมดในรูปแบบที่อ่านง่าย -->
      <button class="language-button">{language.map(lang => lang === 'Thai' ? 'TH' : 'EN').join(' & ')}</button>
      <div class="action-buttons">
        <button class="clear-button" on:click={handleClear}>Clear</button>
        <button class="reconvert-button">Re-convert</button>
      </div>
    </div>
  </div>
  
  <style>
    .result-container {
      border: 1px solid #ddd;
      border-radius: 12px; /* เพิ่มขอบมน */
      padding: 15px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
      background-color: #ffffff; /* พื้นหลังสีขาว */
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1); /* เพิ่มเงา */
      max-width: 320px; /* จำกัดความกว้าง */
      margin: 10px auto; /* จัดให้อยู่กลางหน้า */
    }
  
    .image-container {
      width: 120px;
      height: 120px;
      overflow: hidden;
      border-radius: 10px; /* เพิ่มขอบมนของภาพ */
      border: 1px solid #ccc;
    }
  
    .result-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  
    .result-text {
      width: 95%;
      height: 70px;
      resize: none;
      padding: 8px;
      font-size: 14px;
      border: 1px solid #ddd;
      border-radius: 8px; /* เพิ่มขอบมนของช่องข้อความ */
    }
  
    .button-group {
      display: flex;
      gap: 10px;
      align-items: center;
    }
  
    .language-button {
      border: none;
      padding: 6px 12px;
      cursor: default;
      font-size: 14px;
      background-color: #e0e0e0;
      border-radius: 5px;
      text-transform: uppercase; /* แสดงภาษาเป็นตัวพิมพ์ใหญ่ */
    }
  
    .action-buttons {
      display: flex;
      gap: 6px;
    }
  
    .clear-button,
    .reconvert-button {
      padding: 6px 12px;
      border: 1px solid #000;
      background-color: #ffffff;
      cursor: pointer;
      border-radius: 6px; /* เพิ่มขอบมน */
      transition: background-color 0.3s;
    }
  
    .clear-button:hover,
    .reconvert-button:hover {
      background-color: #f2f2f2; /* สีเมื่อเอาเมาส์ไปวาง */
    }
  </style>      