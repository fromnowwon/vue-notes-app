<script setup>
import { ref } from "vue";
import Card from "./components/Card.vue";
import Modal from "./components/Modal.vue";

const showModal = ref(false);
const notes = ref([]);

const addNote = (newNote) => {
	notes.value.push(newNote);
};

const deleteNote = (id) => {
	notes.value = notes.value.filter((note) => note.id !== id);
};

const updateNote = (updatedNote) => {
	const index = notes.value.findIndex((note) => note.id === updatedNote.id);
	if (index !== -1) {
		notes.value[index] = updatedNote;
	}
};
</script>

<template>
	<main>
		<Modal v-model:show-modal="showModal" @add-note="addNote" />

		<div class="container">
			<header>
				<h1>Notes</h1>
				<button @click="showModal = true">+</button>
			</header>
			<div class="cards-container">
				<Card
					v-for="note in notes"
					:key="note.id"
					:note="note"
					@delete="deleteNote"
					@update="updateNote"
				/>
			</div>
		</div>
	</main>
</template>

<style scoped></style>
