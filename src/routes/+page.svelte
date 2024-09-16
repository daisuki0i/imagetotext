<script lang="ts">
  import Hometext from "../components/Hometext.svelte";
  import ImageUpload from "../components/ImageUpload.svelte";
  import ImageResult from "../components/ImageResult.svelte";

  let imageUrls: string[] = [];
  let texts: string[] = [];
  let language: string[] = [];

  // ฟังก์ชันจัดการเมื่อข้อมูลถูกส่งมาจาก ImageUpload
  function handleConvert(
    uploadedImageUrls: string[],
    uploadedTexts: string[],
    selectedLanguage: string[]
  ) {
    imageUrls = uploadedImageUrls;
    texts = uploadedTexts;
    language = selectedLanguage;
  }

  function clearResult() {
    imageUrls = [];
    texts = [];
    language = [];
  }

</script>

<div class="page-container">
  <Hometext />
  <ImageUpload {handleConvert} />
  {#each imageUrls as imageUrl, index}
    <ImageResult {imageUrl} text={texts[index]} {language} {clearResult} />
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