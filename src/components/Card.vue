<script setup>
import { defineProps, defineEmits, ref } from "vue";

const props = defineProps({
	note: {
		type: Object,
		required: true,
	},
});

const emits = defineEmits(["delete", "update"]);

const deleteNote = () => {
	emits("delete", props.note.id);
};

const editedText = ref(props.note.text);
const isEditing = ref(false);

const toggleEdit = () => {
	isEditing.value = !isEditing.value;
	showMenu.value = false;
};

const saveEdit = () => {
	const updatedNote = { ...props.note, text: editedText.value };
	emits("update", updatedNote);
	isEditing.value = false;
};

const cancelEdit = () => {
	isEditing.value = false;
	editedText.value = props.note.text;
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

const showMenu = ref(false);

const hadleToggleMenu = () => {
	showMenu.value = !showMenu.value;
};
</script>

<template>
	<div>
		<div class="card" :style="{ backgroundColor: note.backgroundColor }">
			<template v-if="!isEditing">
				<p class="main-text">{{ note.text }}</p>
				<p class="date">{{ formatDate(note.date) }}</p>
				<div class="toggle-menu">
					<button class="toggle-menu-btn" @click="hadleToggleMenu">⁝</button>
					<div class="toggle-menu-list" v-show="showMenu">
						<ul>
							<li @click="toggleEdit">수정</li>
							<li @click="deleteNote">삭제</li>
						</ul>
					</div>
				</div>
			</template>
			<template v-else>
				<textarea
					v-model="editedText"
					rows="10"
					cols="30"
					id="editTextArea"
					name="editTextArea"
					class="edit-text"
				></textarea>
				<div class="btn-box">
					<button class="save-btn btn-underline" @click.stop="saveEdit">
						저장
					</button>
					<button class="cancel-btn btn-underline" @click.stop="cancelEdit">
						취소
					</button>
				</div>
			</template>
		</div>
	</div>
</template>

<style scoped>
.card {
	display: flex;
	justify-content: space-between;
	flex-direction: column;
	position: relative;
	width: 225px;
	min-height: 225px;
	padding: 40px 20px 20px;
	margin: 0 20px 20px 0;
	background-color: rgb(237, 182, 44);
	border-radius: 15px;
}

.main-text {
	font-size: 1rem;
	white-space: normal;
	word-wrap: break-word;
	overflow-wrap: break-word;
}

.date {
	font-weight: 700;
	font-size: 0.75rem;
}

/* Toggle Menu */
.toggle-menu {
	display: flex;
	align-items: center;
	justify-content: center;
	position: absolute;
	right: 10px;
	top: 10px;
	width: 20px;
	height: 20px;
}

.toggle-menu-btn {
	width: 100%;
	height: 100%;
	margin: 0;
	padding: 0;
	font-size: 30px;
	background-color: transparent;
	cursor: pointer;
}

.toggle-menu-list {
	position: absolute;
	right: -2px;
	top: 35px;
	width: 40px;
	background-color: aliceblue;
}

.toggle-menu-list li {
	margin: 2px 0;
	font-size: 0.875rem;
	text-align: center;
	background-color: aliceblue;
	cursor: pointer;
}

.toggle-menu-list li + li {
	border-top: 1px solid #fff;
}
</style>
