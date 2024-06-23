<script setup>
import { ref } from "vue";

const showModal = ref(false);
const notes = ref([]);
const newNote = ref("");

const addNote = () => {
	notes.value.push({
		id: Math.floor(Math.random() * 1000000),
		text: newNote.value,
		date: new Date(),
	});
	showModal.value = false;
	newNote.value = "";
};
</script>

<template>
	<main>
		<div class="overlay" v-show="showModal">
			<div class="modal">
				<textarea
					v-model.trim="newNote"
					name="note"
					id="note"
					rows="10"
					cols="30"
				></textarea>
				<div class="btn-box">
					<button class="add-btn" @click="addNote">추가</button>
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
				<div class="card" v-for="note in notes" :key="note.id">
					<p class="main-text">{{ note.text }}</p>
					<p class="date">{{ note.date }}</p>
				</div>
			</div>
		</div>
	</main>
</template>

<style scoped></style>
