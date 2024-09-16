<script>
  // ลบส่วนที่ไม่จำเป็น เช่น การ import รูปภาพ และ component อื่น ๆ ออกไป
</script>

<svelte:head>
  <title>Image to Text</title>
  <meta name="description" content="Image Text Processing" />
  <link
    href="https://fonts.googleapis.com/css2?family=Jomhuria&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<section>
  <h1 class="header">
    IMAGE <span style="color: #000000;">to</span>
    <span style="color: #1D8B92;">
      TEXT
      <img
        src="/English.png"
        alt="English Flag"
        style="width: 30px; height: 20px; vertical-align: middle;"
      />
      <img
        src="/Thai.png"
        alt="Thai Flag"
        style="width: 30px; height: 17px; vertical-align: middle;"
      />
    </span>
  </h1>

  <h2 class="title">Image Text Processing</h2>
  <p class="headline">
    Image Text Processing is an AI for converting a picture to text so people
    can use text for searching similar picture or topic.
  </p>

  <h2 class="title">How to use</h2>
  <p class="headline">
    Insert picture on an "Insert Picture" box. Then, select the language you
    prefer. Finally click the "Convert" button for converting the picture to
    text.
  </p>
</section>

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: flex-start; /* เปลี่ยนจาก center เป็น flex-start */
    align-items: center;
    text-align: center;
    padding: 20px;
  }

  h1,
  h2 {
    margin: 0;
  }

  p {
    margin: 10px 0;
  }

  .header {
    font-size: 3rem;
    font-weight: bold;
    color: #c22020;
    font-family: "Kanit", sans-serif;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  }

  .title {
    font-size: 2rem;
    margin-top: 20px;
    font-family: "Jomhuria", sans-serif;
    color: #000000;
  }

  .headline {
    font-size: 1.5rem;
    text-align: center;
    max-width: 600px;
    font-family: "Jomhuria", sans-serif;
    color: #000000;
  }
</style>
