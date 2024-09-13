<script>
	// ไม่มีการนำเข้าเพิ่มเติมเนื่องจาก Header ถูกลบไปแล้ว
</script>

<div class="app">
	<!-- ลบการเรียกใช้ Header ที่ไม่มีอยู่แล้ว -->
	<main>
		<slot />
	</main>

	<footer>
		<p style="font-size: 1.5rem; text-align: center; max-width: 600px; font-family: 'Jomhuria', sans-serif; color: #000000">
			This website is created for <span style="color: #C22020;">AI Projects</span> only
		</p>

	</footer>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
</style>
