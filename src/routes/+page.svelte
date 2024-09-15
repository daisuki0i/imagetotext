<script>
    // นำเข้า Hometext.svelte และ ImageUpload.svelte เพื่อใช้งานในหน้า +page.svelte
    import Hometext from '../components/Hometext.svelte';
    import ImageUpload from '../components/ImageUpload.svelte'; // ตรวจสอบเส้นทางให้ถูกต้องตามที่เก็บไฟล์
</script>

<!-- แสดง Hometext และ ImageUpload ใน container ที่จัดวางให้อยู่ตรงกลาง -->
<div class="page-container">
    <Hometext />
    <ImageUpload />
</div>

<style>
    .page-container {
        display: flex;
        flex-direction: column; /* จัดวางในแนวตั้ง */
        justify-content: center; /* จัดให้อยู่กลางในแนวนอน */
        align-items: center; /* จัดให้อยู่กลางในแนวตั้ง */
        min-height: 100vh; /* ใช้พื้นที่เต็มหน้าจอแนวตั้ง */
        overflow: auto; /* ทำให้เลื่อนขึ้นลงได้ */
        padding: 20px; /* เพิ่มระยะห่าง */
        box-sizing: border-box; /* รวม padding ในขนาดของ container */
    }
</style>

