<script setup>
import { defineProps, defineEmits, ref } from "vue";

const props = defineProps({
	note: {
		type: Object,
		required: true,
	},
});

const emits = defineEmits(["delete"]);

const deleteNote = () => {
	emits("delete", props.note.id);
};

const editedText = ref(props.note.text);
const isEditing = ref(false);

const toggleEdit = () => {
	isEditing.value = !isEditing.value;
};

const saveEdit = () => {
	const updatedNote = { ...props.note, text: editedText.value };
	emits("update", updatedNote);
	isEditing.value = false;
};

const cancelEdit = () => {
	isEditing.value = false;
};

const formatDate = (date) => {
	const options = {
		year: "numeric",
		month: "2-digit",
		day: "2-digit",
		hour: "2-digit",
		minute: "2-digit",
		second: "2-digit",
		hour12: false,
		timeZone: "Asia/Seoul",
	};

	return new Intl.DateTimeFormat("ko-KR", options).format(date);
};
</script>

<template>
	<div class="card">
		<template v-if="!isEditing">
			<p class="main-text">{{ note.text }}</p>
			<p class="date">{{ formatDate(note.date) }}</p>
			<button class="edit-btn" @click="toggleEdit">수정</button>
			<button class="delete-btn" @click="deleteNote">삭제</button>
		</template>
		<template v-else>
			<textarea v-model="editedText"></textarea>
			<button class="save-btn" @click="saveEdit">저장</button>
			<button class="cancel-btn" @click="cancelEdit">취소</button>
		</template>
	</div>
</template>
