<script lang="ts">
  export let handleConvert: (
    uploadedImageUrl: string,
    uploadedText: string,
    selectedLanguage: string[]
  ) => void;

  let file: File | null = null; // เก็บไฟล์ที่อัปโหลด
  let selectedLanguage: string[] = []; // เก็บภาษาที่เลือกเป็น array
  let showUploadOptions: boolean = false; // ควบคุมการแสดง pop-up อัปโหลด
  let imageUrl: string | null = null; // เก็บ URL ของรูปภาพที่อัปโหลด

  // ฟังก์ชันสำหรับจัดการอัปโหลดไฟล์
  function handleFileUpload(event: Event) {
    const input = event.target as HTMLInputElement;
    if (input.files && input.files[0]) {
      file = input.files[0];
      imageUrl = URL.createObjectURL(file); // สร้าง URL สำหรับรูปภาพ
      console.log("File uploaded:", file.name);
      showUploadOptions = false; // ปิด pop-up หลังจากอัปโหลด
    }
  }

  // ฟังก์ชันเปิดตัวเลือกอัปโหลด
  function openUploadPopup() {
    showUploadOptions = true;
  }

  // ฟังก์ชันสำหรับแปลงภาพเมื่อกดปุ่ม Convert
  function convertImage() {
    if (!file) {
      alert("Please upload an image first.");
      return;
    }

    if (selectedLanguage.length === 0) {
      alert("Please select at least one language before converting.");
      return;
    }

    if (!imageUrl) {
      alert("Image URL is not available.");
      return;
    }

    // ส่งข้อมูลกลับไปที่ +page.svelte โดยตรวจสอบให้ imageUrl เป็น string เสมอ
    handleConvert(
      imageUrl || "",
      "Generated text from backend",
      selectedLanguage
    );
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
</script>

<div class="upload-container">
  <!-- แสดงชื่อไฟล์ที่อัปโหลด -->
  {#if file}
    <p class="file-name">Filename: {file.name}</p>
  {/if}

  <!-- กรอบสำหรับการอัปโหลดไฟล์ -->
  <button
    type="button"
    class="dropzone"
    on:click={openUploadPopup}
    aria-label="Upload Image"
  >
    {#if imageUrl}
      <img src={imageUrl} alt="Uploaded Image" class="uploaded-image" />
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
    <button class="convert-button" on:click={convertImage}>Convert</button>
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

  .file-name {
    margin: 0;
    font-size: 14px;
    font-weight: normal;
  }

  .dropzone {
    border: 2px dashed #000;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
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

  .uploaded-image {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
    border: none;
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