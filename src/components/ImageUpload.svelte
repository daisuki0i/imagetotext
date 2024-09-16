<script lang="ts">
  import { onMount } from "svelte";
  import Cookies from "js-cookie"; // ใช้ js-cookie สำหรับจัดการคุกกี้

  export let handleConvert: (
    uploadedImageUrls: string[], // รับ URL รูปภาพหลายรายการ
    uploadedTexts: string[], // รับข้อความหลายรายการ
    selectedLanguage: string[] // รับภาษาหลายภาษา
  ) => void;

  let files: File[] = []; // เก็บไฟล์ที่อัปโหลดทั้งหมดในรูปแบบ array
  let selectedLanguage: string[] = []; // เก็บภาษาที่เลือกเป็น array
  let showUploadOptions: boolean = false; // ควบคุมการแสดง pop-up อัปโหลด
  let imageUrls: string[] = []; // เก็บ URL ของรูปภาพที่อัปโหลด

  let currentOutput: string = "";

  // ฟังก์ชันสำหรับจัดการอัปโหลดไฟล์
  function handleFileUpload(event: Event) {
    const input = event.target as HTMLInputElement;
    if (input.files && input.files.length > 0) {
      files = Array.from(input.files); // แปลง FileList เป็น array
      imageUrls = files.map((file) => URL.createObjectURL(file)); // สร้าง URL สำหรับรูปภาพแต่ละไฟล์

      // คอมเมนต์ส่วนที่เกี่ยวข้องกับการใช้งานคุกกี้
      // imageUrls.forEach((url, index) => {
      //   Cookies.set(`uploadedImageUrl_${index}`, url, { expires: 7 }); // เก็บ URL ของรูปภาพในคุกกี้
      // });

      showUploadOptions = false; // ปิด pop-up หลังจากอัปโหลด
    }
  }

  // ฟังก์ชันเปิดตัวเลือกอัปโหลด
  function openUploadPopup() {
    showUploadOptions = true;
  }

  // ฟังก์ชันสำหรับแปลงภาพเมื่อกดปุ่ม Convert
  async function convertImages() {
    if (files.length === 0) {
      alert("Please upload at least one image.");
      return;
    }

    if (selectedLanguage.length === 0) {
      alert("Please select at least one language before converting.");
      return;
    }

    const formData = new FormData();
    // formData.append("file", files[0]);

    files.forEach((file, index) => {
      formData.append(`file_${index}`, file);
    });

    // console.log(formData);
    try {
      const response = await fetch("http://127.0.0.1:5000/upload", {
        method: "POST",
        body: formData,
      });

      const result = await response.json();
      currentOutput = result;
      let arrayCurrentOutput = Object.values(currentOutput);
      console.log(typeof arrayCurrentOutput);
      console.log(arrayCurrentOutput);

      handleConvert(
        imageUrls,
        // Array(files.length).fill(currentOutput),
        arrayCurrentOutput,
        selectedLanguage
      );
    } catch (error) {
      console.error("Error converting images:", error);
    }

    // handleConvert(
    //   imageUrls,
    //   Array(files.length).fill(currentOutput),
    //   // currentOutput,

    //   selectedLanguage
    // );

    // เคลียร์ภาพที่ถูกอัปโหลดหลังการแปลง
    files = [];
    imageUrls = [];
  }

  // ฟังก์ชันสำหรับปิด pop-up เมื่อกดปุ่ม Cancel
  function closeUploadPopup() {
    showUploadOptions = false;
  }

  // ฟังก์ชันจัดการการเปลี่ยนแปลงภาษา
  function handleLanguageChange(event: Event) {
    const input = event.target as HTMLInputElement;
    if (input.checked) {
      selectedLanguage.push(input.value);
    } else {
      selectedLanguage = selectedLanguage.filter(
        (lang) => lang !== input.value
      );
    }
  }

  // คอมเมนต์ส่วนที่เกี่ยวข้องกับการโหลด URL ของรูปภาพจากคุกกี้เมื่อหน้าเว็บถูกเปิด
  // onMount(() => {
  //   let index = 0;
  //   let savedImageUrl = Cookies.get(`uploadedImageUrl_${index}`);
  //   while (savedImageUrl) {
  //     imageUrls.push(savedImageUrl);
  //     index++;
  //     savedImageUrl = Cookies.get(`uploadedImageUrl_${index}`);
  //   }
  // });
</script>

<div class="upload-container">
  <!-- แสดงจำนวนและชื่อไฟล์ เฉพาะเมื่อมีไฟล์ที่ถูกอัปโหลด -->
  {#if files.length > 0}
    <div class="file-name-container">
      <strong>Uploaded {files.length} files:</strong>
      {#each files as file, index}
        <span class="file-name"
          >{file.name}{index < files.length - 1 ? "," : ""}</span
        >
      {/each}
    </div>
  {/if}

  <!-- กรอบสำหรับการอัปโหลดไฟล์ -->
  <button
    type="button"
    class="dropzone"
    on:click={openUploadPopup}
    aria-label="Upload Image"
  >
    {#if imageUrls.length > 0}
      <!-- แทนที่ส่วนนี้ด้วยโครงสร้างใหม่ที่จัดการไว้ -->
      <div class="image-gallery">
        {#each imageUrls as url}
          <img src={url} alt="Uploaded Image" class="uploaded-image" />
        {/each}
      </div>
    {:else}
      <p>Insert Image Here</p>
    {/if}
  </button>

  {#if showUploadOptions}
    <!-- Pop-up สำหรับการอัปโหลด -->
    <div class="upload-popup">
      <input
        id="fileInput"
        type="file"
        accept="image/*"
        multiple
        on:change={handleFileUpload}
      />
      <button type="button" on:click={closeUploadPopup}>Cancel</button>
    </div>
  {/if}

  <!-- Container สำหรับปุ่ม -->
  <div class="button-container">
    <!-- ปุ่มเลือกภาษา -->
    <div class="language-options">
      <label>
        <input type="checkbox" value="Thai" on:change={handleLanguageChange} /> Thai
      </label>
      <label>
        <input
          type="checkbox"
          value="English"
          on:change={handleLanguageChange}
        /> English
      </label>
    </div>

    <!-- ปุ่ม Convert -->
    <button class="convert-button" on:click={convertImages}>Convert</button>
  </div>
</div>

<style>
  .upload-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    margin-top: 10px;
  }

  .file-name-container {
    display: flex;
    flex-wrap: wrap; /* ให้ข้อความจัดเรียงต่อไปบรรทัดใหม่เมื่อเกินพื้นที่ */
    gap: 5px;
    max-width: 480px; /* กำหนดความกว้างสูงสุดให้เท่ากับกรอบที่อัปโหลดรูปภาพ */
    margin-bottom: 10px; /* เพิ่มระยะห่างระหว่างชื่อไฟล์กับกรอบ */
  }

  strong {
    font-size: 14px;
    color: #333; /* ปรับสีข้อความเพื่อให้ดูชัดเจน */
  }

  .file-name {
    font-size: 14px;
    color: #333; /* ปรับสีข้อความเพื่อให้ดูชัดเจน */
    word-break: break-all; /* ตัดข้อความเมื่อยาวเกินไป */
  }

  .dropzone {
    border: 2px dashed #000;
    border-radius: 10px;
    display: flex;
    flex-wrap: wrap; /* ทำให้สามารถแสดงหลายรูปในกรอบเดียวกัน */
    align-items: center;
    justify-content: center;
    padding: 20px;
    cursor: pointer;
    width: 500px;
    height: 150px;
    background-color: #f9f9f9;
    position: relative;
    overflow: hidden;
  }

  .image-gallery {
    display: flex;
    flex-wrap: wrap; /* จัดการการแสดงผลเมื่อภาพมีจำนวนมาก */
    gap: 10px; /* เว้นระยะห่างระหว่างภาพ */
    justify-content: center; /* จัดเรียงภาพให้อยู่กลาง */
    max-width: 500px; /* กำหนดความกว้างสูงสุดเพื่อให้ภาพอยู่ในกรอบ */
  }

  .uploaded-image {
    width: 100px; /* กำหนดความกว้างของภาพให้เท่ากัน */
    height: 100px; /* กำหนดความสูงของภาพให้เท่ากัน */
    object-fit: cover; /* จัดการภาพให้ครอบคลุมกรอบ */
    border: 1px solid #ccc; /* เพิ่มขอบเพื่อความสวยงาม */
    border-radius: 5px; /* เพิ่มความโค้งมนให้ภาพ */
  }

  .upload-popup {
    position: absolute;
    transform: translateY(50%);
    background: white;
    border: 1px solid #ccc;
    padding: 10px;
    display: flex;
    flex-direction: column;
    gap: 5px;
    z-index: 10;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
  }

  .button-container {
    display: flex;
    gap: 10px;
    justify-content: space-between;
    align-items: center;
    margin-top: 1px;
    width: 100%;
    max-width: 500px;
  }

  .convert-button {
    padding: 10px 20px;
    border: 1px solid #000;
    background-color: #fff;
    color: #000;
    font-weight: bold;
    cursor: pointer;
    border-radius: 5px;
  }
  .convert-button:hover {
    background-color: #f0f0f0;
  }

  .language-options {
    display: flex;
    gap: 10px;
    align-items: center;
    margin-right: auto;
  }

  label {
    display: flex;
    align-items: center;
    font-size: 14px;
  }
</style>
