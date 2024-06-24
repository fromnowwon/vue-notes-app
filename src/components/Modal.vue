<script setup>
import { ref, defineProps, defineEmits } from "vue";

const props = defineProps({
	showModal: Boolean,
});

const emits = defineEmits(["add-note", "update:show-modal"]);

const newNote = ref("");
const errorMessage = ref("");
const palette = [
	"hsl(0, 100%, 75%)",
	"hsl(60, 100%, 75%)",
	"hsl(120, 100%, 75%)",
	"hsl(180, 100%, 75%)",
	"hsl(240, 100%, 75%)",
	"hsl(300, 100%, 75%)",
];

const addNote = () => {
	if (!newNote.value.trim())
		return (errorMessage.value = "내용을 입력해주세요!");

	const selectedColorValue = selectedColor.value || getRandomColor();

	emits("add-note", {
		id: Math.floor(Math.random() * 1000000),
		text: newNote.value.trim(),
		date: new Date(),
		backgroundColor: selectedColorValue,
	});

	closeModal();
};

const closeModal = () => {
	emits("update:show-modal", false);
	newNote.value = "";
	errorMessage.value = "";
	selectedColor.value = "";
};

const selectedColor = ref("");

function getRandomColor() {
	return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const selectColor = (color) => {
	selectedColor.value = color;
};
</script>

<template>
	<div class="overlay" v-show="showModal">
		<div class="modal">
			<h2 class="tit">새 메모</h2>
			<div class="color-palette">
				<div
					v-for="color in palette"
					:key="color"
					:style="{ backgroundColor: color }"
					@click="selectColor(color)"
				>
					<span v-if="selectedColor === color" class="check-mark">✓</span>
				</div>
			</div>
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
				<button class="add-btn btn-primary" @click="addNote">추가</button>
				<button class="close-btn btn-secondary" @click="closeModal">
					취소
				</button>
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
}

.error-message {
	font-size: 0.875rem;
	color: red;
}

.color-palette {
	display: flex;
	align-items: center;
	margin-bottom: 10px;
}

.color-palette div {
	position: relative;
	width: 27px;
	height: 27px;
	border-radius: 50%;
	cursor: pointer;
	margin-right: 5px;
}

.color-palette div:last-child {
	margin-right: 0;
}

.check-mark {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	color: white;
	font-size: 18px;
}
</style>
