<script setup>
import { defineProps, defineEmits } from "vue";

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
		<p class="main-text">{{ note.text }}</p>
		<p class="date">{{ formatDate(note.date) }}</p>
		<button class="delete-btn" @click="deleteNote">X</button>
	</div>
</template>
