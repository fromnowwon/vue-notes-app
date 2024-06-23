<script setup>
import { ref } from "vue";
import Card from "./components/Card.vue";

const showModal = ref(false);
const notes = ref([]);
const newNote = ref("");
const errorMessage = ref("");

const addNote = () => {
	if (!newNote.value.length)
		return (errorMessage.value = "메모를 입력해주세요!!");

	notes.value.push({
		id: Math.floor(Math.random() * 1000000),
		text: newNote.value,
		date: new Date(),
	});

	showModal.value = false;
	newNote.value = "";
	errorMessage.value = "";
};

const closeModal = () => {
	showModal.value = false;
	newNote.value = "";
	errorMessage.value = "";
};

const deleteNote = (id) => {
	notes.value = notes.value.filter((note) => note.id !== id);
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
				<p class="error-message" v-if="errorMessage">{{ errorMessage }}</p>
				<div class="btn-box">
					<button class="add-btn" @click="addNote">추가</button>
					<button class="close-btn" @click="closeModal">취소</button>
				</div>
			</div>
		</div>
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
				/>
			</div>
		</div>
	</main>
</template>

<style scoped></style>
