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
				<button @click="showModal = true" class="add-btn">+</button>
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

<style scoped>
main {
	width: 100vw;
	height: 100vh;
}

.container {
	max-width: 1000px;
	padding: 10px;
	margin: 0 auto;
}

header {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

h1 {
	font-size: 7rem;
}

.cards-container {
	display: flex;
	flex-wrap: wrap;
}

.add-btn {
	width: 50px;
	height: 50px;
	padding: 0;
	background-color: #333;
	font-size: 34px;
	color: #fff;
	border-radius: 50%;
}

.add-btn:hover {
	background-color: aquamarine;
}
</style>
