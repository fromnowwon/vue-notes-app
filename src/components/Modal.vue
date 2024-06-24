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
			<h2 class="tit">새 메모</h2>
			<textarea
				v-model.trim="newNote"
				name="note"
				id="note"
				class="note-area"
				rows="10"
				cols="30"
				placeholder="내용을 입력해주세요"
			></textarea>
			<p class="error-message" v-if="errorMessage">{{ errorMessage }}</p>
			<div class="btn-box">
				<button class="add-btn" @click="addNote">추가</button>
				<button class="close-btn" @click="closeModal">취소</button>
			</div>
		</div>
	</div>
</template>

<style scoped>
.overlay {
	display: flex;
	justify-content: center;
	align-items: center;
	position: absolute;
	left: 0;
	top: 0;
	width: 100%;
	height: 100%;
	background-color: rgba(0, 0, 0, 0.5);
	z-index: 10;
}

.modal {
	width: 400px;
	padding: 1rem;
	border-radius: 5px;
	background-color: #fff;
	box-shadow: 1px 2px 10px 1px rgba(0, 0, 0, 0.3);
}

.tit {
	margin-bottom: 5px;
	font-size: 1rem;
}

.note-area {
	width: 100%;
	padding: 10px;
	border: 1px solid #bbb;
	border-radius: 5px;
}

.btn-box {
	display: flex;
	align-items: center;
	justify-content: center;
	margin-top: 5px;
}

.btn-box button {
	border: 0;
	font-size: 0.875rem;
	margin: 0 5px;
	padding: 4px 14px;
	border-radius: 3px;
	cursor: pointer;
}

.add-btn {
	background-color: #333;
	color: #fff;
}

.close-btn {
	background-color: #e5e5e5;
}
</style>
