<script lang="ts">
  import Hometext from "../components/Hometext.svelte";
  import ImageUpload from "../components/ImageUpload.svelte";
  import ImageResult from "../components/ImageResult.svelte";

  let imageUrls: string[] = [];
  let texts: string[] = [];
  let languages: string[][] = []; // เก็บข้อมูลภาษาของแต่ละบล็อคแยกกัน

  // ฟังก์ชันจัดการเมื่อข้อมูลถูกส่งมาจาก ImageUpload
  function handleConvert(
    uploadedImageUrls: string[],
    uploadedTexts: string[],
    selectedLanguages: string[]
  ) {
    imageUrls = uploadedImageUrls;
    texts = uploadedTexts;
    languages = uploadedImageUrls.map(() => selectedLanguages); // เก็บภาษาสำหรับแต่ละบล็อค
  }

  // ฟังก์ชันจัดการการลบเฉพาะบล็อคที่ถูกกด clear
  function clearResult(index: number) {
    imageUrls = [...imageUrls.slice(0, index), ...imageUrls.slice(index + 1)];
    texts = [...texts.slice(0, index), ...texts.slice(index + 1)];
    languages = [...languages.slice(0, index), ...languages.slice(index + 1)]; // ลบข้อมูลภาษาของบล็อคนั้นด้วย
  }
</script>

<div class="page-container">
  <Hometext />
  <ImageUpload {handleConvert} />
  {#each imageUrls as imageUrl, index}
    <ImageResult {imageUrl} text={texts[index]} language={languages[index]} clearResult={() => clearResult(index)} />
  {/each}
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


