<script lang="ts">
  import Hometext from "../components/Hometext.svelte";
  import ImageUpload from "../components/ImageUpload.svelte";
  import ImageResult from "../components/ImageResult.svelte";

  let imageUrl: string | null = null;
  let text: string = "";
  let language: string[] = []; // เปลี่ยนเป็น array เพื่อรับค่าภาษาหลายภาษา

  // ฟังก์ชันสำหรับรับข้อมูลจาก ImageUpload เมื่อกดปุ่ม Convert
  function handleConvert(uploadedImageUrl: string, uploadedText: string, selectedLanguage: string[]) {
    imageUrl = uploadedImageUrl;
    text = uploadedText;
    language = selectedLanguage;
  }

  // ฟังก์ชันสำหรับรีเซ็ตค่าของ pop-up เมื่อกดปุ่ม Clear
  function clearResult() {
    imageUrl = null;
    text = "";
    language = [];
  }
</script>

<div class="page-container">
  <Hometext />
  <ImageUpload {handleConvert} />
  {#if imageUrl}
    <ImageResult {imageUrl} {text} {language} {clearResult} />
  {/if}
</div>

<style>
  .page-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    overflow: auto;
    padding: 20px;
    box-sizing: border-box;
  }
</style>