<script setup>
import { ref, defineProps, defineEmits } from "vue";

const props = defineProps({
	showModal: Boolean,
});

const emits = defineEmits(["add-note", "update:show-modal"]);

const newNote = ref("");
const errorMessage = ref("");

const addNote = () => {
	if (!newNote.value.trim())
		return (errorMessage.value = "메모를 입력해주세요!!");

	emits("add-note", {
		id: Math.floor(Math.random() * 1000000),
		text: newNote.value.trim(),
		date: new Date(),
	});

	closeModal();
};

const closeModal = () => {
	emits("update:show-modal", false);
	newNote.value = "";
	errorMessage.value = "";
};
</script>

<template>
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
</template>
