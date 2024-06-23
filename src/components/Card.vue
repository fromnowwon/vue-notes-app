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
</script>

<template>
	<div class="card">
		<p class="main-text">{{ note.text }}</p>
		<p class="date">{{ note.date }}</p>
		<button class="delete-btn" @click="deleteNote">X</button>
	</div>
</template>
