<script lang="ts">
  let file: File | null = null; // เก็บไฟล์ที่อัปโหลด
  let selectedLanguage: string = ""; // เก็บภาษาที่เลือก
  let showUploadOptions: boolean = false; // ควบคุมการแสดง pop-up อัปโหลด

  // ฟังก์ชันสำหรับจัดการอัปโหลดไฟล์
  function handleFileUpload(event: Event) {
    const input = event.target as HTMLInputElement;
    if (input.files && input.files[0]) {
      file = input.files[0];
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
    alert(`Converting image: ${file.name}`);
  }

  // ฟังก์ชันสำหรับการดาวน์โหลด
  function downloadFile() {
    alert("Download button clicked.");
  }

  // ฟังก์ชันสำหรับปิด pop-up เมื่อกดปุ่ม Cancel
  function closeUploadPopup() {
    showUploadOptions = false;
  }
</script>

<div class="upload-container">
  <!-- กรอบสำหรับการอัปโหลดไฟล์ -->
  <button
    type="button"
    class="dropzone"
    on:click={openUploadPopup}
    aria-label="Upload Image"
  >
    <p>Insert Image Here</p>
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
      <!-- ปิด pop-up เมื่อกดปุ่ม Cancel -->
    </div>
  {/if}

  <!-- Container สำหรับปุ่ม -->
  <div class="button-container">
    <!-- ปุ่มดาวน์โหลด -->
    <button class="download-button" on:click={downloadFile}>Download</button>

    <!-- ปุ่มเลือกภาษา -->
    <div class="language-options">
      <label>
        <input type="checkbox" value="Thai" bind:group={selectedLanguage} /> Thai
      </label>
      <label>
        <input type="checkbox" value="English" bind:group={selectedLanguage} /> English
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
    gap: 20px;
    margin-top: 10px;
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
    justify-content: space-between; /* จัดปุ่มให้อยู่คนละด้าน */
    align-items: center;
    margin-top: 10px;
    width: 100%; /* ทำให้ปุ่มเต็มพื้นที่กรอบ */
    max-width: 500px; /* จำกัดความกว้างให้เท่ากับกรอบ */
  }

  .download-button {
    padding: 10px 20px;
    border: 1px solid #000;
    background-color: #fff;
    color: #000;
    font-weight: bold;
    cursor: pointer;
    border-radius: 5px;
    margin-left: 0; /* ปรับตำแหน่งปุ่มไปทางซ้ายให้ใกล้ขอบกรอบ */
  }

  .convert-button {
    padding: 10px 20px;
    border: 1px solid #000;
    background-color: #fff;
    color: #000;
    font-weight: bold;
    cursor: pointer;
    border-radius: 5px;
    margin-right: 0; /* จัดปุ่มให้อยู่ชิดขวาของกรอบ */
  }

  .download-button:hover,
  .convert-button:hover {
    background-color: #f0f0f0;
  }

  .language-options {
    display: flex;
    gap: 10px;
    align-items: center;
  }

  label {
    display: flex;
    align-items: center;
    font-size: 14px;
  }
</style>

  
