<script setup>
import { ref } from "vue";

const showModal = ref(false);
</script>

<template>
	<main>
		<div class="overlay" v-show="showModal">
			<div class="modal">
				<textarea name="note" id="note" rows="10" cols="30"></textarea>
				<div class="btn-box">
					<button class="add-btn">추가</button>
					<button class="close-btn" @click="showModal = false">취소</button>
				</div>
			</div>
		</div>
		<div class="container">
			<header>
				<h1>Notes</h1>
				<button @click="showModal = true">+</button>
			</header>
			<div class="cards-container">
				<div class="card">
					<p class="main-text"></p>
					<p class="date"></p>
				</div>
			</div>
		</div>
	</main>
</template>

<style scoped></style>
