<script setup>
import { InfoFilled } from "@element-plus/icons-vue"

const router = useRouter()
const route = useRoute()

const tasks = ref([])
const task = ref({
	id: 0,
	description: "",
	isCompleted: false,
})

const getTodos = () => {
	tasks.value = JSON.parse(localStorage.getItem("tasks"))
}

const getTask = () => {
	task.value = tasks.value.find(task => task.id === +route.params.id)
}

getTodos()
getTask()

const saveTask = () => {
	tasks.value = tasks.value.map(currentTask => {
		if (currentTask.id === +route.params.id) {
			currentTask.description = task.value.description
		}
		return currentTask
	})

	localStorage.setItem("tasks", JSON.stringify(tasks.value))

	router.push({ name: "index" })
}

const cancelEditTask = () => {
	router.push({ name: "index" })
}
</script>

<template>
	<div class="container--edit">
		<h2>Редактировать задачу</h2>
		<label class="edit__label" for="edit-task">Название задачи</label>
		<input
			v-model="task.description"
			class="edit__input"
			type="text"
			placeholder=""
			id="edit-task"
		/>
		<!-- <div class="edit__container"> -->
		<el-button-group class="edit__container">
			<!-- <button @click="cancelEditTask" class="edit__cancel" type="button">
				Отменить
			</button>
			<button @click="saveTask" class="edit__save" type="button">
				Сохранить изменения
			</button> -->

			<!-- <el-button
				@click="cancelEditTask"
				class="edit__cancel"
				type="danger"
				plain
			>
				Отменить
			</el-button> -->

			<el-popconfirm
				width="300"
				:icon="InfoFilled"
				icon-color="#ff9900"
				title="Вы уверены, что хотите отменить редактирование задачи задачи?"
				@confirm="cancelEditTask"
			>
				<template #reference>
					<el-button class="edit__cancel" type="danger" plain>
						Отменить
					</el-button>
				</template>
				<template #actions="{ confirm, cancel }">
					<el-button
						@click="cancel"
						plain
						style="width: 50px; border: 2px solid #a0cfff; border-radius: 5px"
						type="primary"
					>
						Нет
					</el-button>
					<el-button
						@click="confirm"
						plain
						style="width: 50px; border: 2px solid #a0cfff; border-radius: 5px"
						type="primary"
					>
						Да
					</el-button>
				</template>
			</el-popconfirm>

			<el-button @click="saveTask" class="edit__save" type="primary" plain>
				Сохранить изменения
			</el-button>
		</el-button-group>

		<!-- </div> -->
	</div>
</template>

<style scoped>
*,
*::before,
*::after {
	padding: 0;
	margin: 0;
	border: 0;
	box-sizing: border-box;
}

.container--edit {
	width: 412px;
	margin: 0 auto;
	padding: 12px;
	background-color: #e6edf9;
	border-bottom: 2px solid #ffffff;
}

.container--edit h2 {
	margin-bottom: 12px;
	font-weight: 400;
	font-style: normal;
	font-size: 24px;

	text-align: center;
}

.edit__label {
	display: block;
	margin-bottom: 8px;
}

.edit__input {
	display: block;

	height: 35px;
	width: 100%;

	padding: 1px 10px;

	margin-bottom: 18px;

	font-style: normal;
	font-weight: 400;
	color: black;

	border: 1px solid #9ca3af;
	border-radius: 5px;
	outline: none;
	background-color: #fefefe;

	transition: all 0.4s ease;
}

.edit__input::placeholder {
	font-style: normal;
	font-weight: 400;
	color: black;
}

.edit__input:hover {
	border-color: #c0c4cc;
}

.edit__input:focus,
.edit__input:active {
	border-color: #a0cfff;
}

.edit__container {
	display: flex;
}

/* .edit__cancel {
	width: 50%;
	height: 32px;

	font-weight: 600;
	color: #f56c93;

	border: 2px solid #facbcb;
	border-radius: 5px;
	background-color: #fef0f0;

	display: flex;
	justify-content: center;
	align-items: center;

	cursor: pointer;

	transition: all 0.3s ease;
}

.edit__cancel:hover,
.edit__cancel:focus,
.edit__cancel:active {
	background-color: #f56c6c;
	color: #fffff6;
	border: 2px solid #f56c6c;
} */

.edit__cancel {
	border: 2px solid #facbcb;
	border-radius: 5px;
	width: 50%;
}

.edit__cancel:hover,
.edit__cancel:focus,
.edit__cancel:active {
	border: 2px solid #f56c6c;
}

/* .edit__save {
	width: 50%;
	height: 32px;

	font-weight: 600;
	color: #40b4ff;

	border: 2px solid #a0cfff;
	border-radius: 5px;
	background-color: #ecf5ff;

	display: flex;
	justify-content: center;
	align-items: center;

	cursor: pointer;

	transition: all 0.3s ease;
}

.edit__save:hover,
.edit__save:focus,
.edit__save:active {
	background-color: #409eff;
	color: #f3ffff;
	border: 2px solid #409eff;
} */

.edit__save {
	border: 2px solid #a0cfff;
	border-radius: 5px;
	width: 50%;
	margin-left: 0;
}

.edit__save:hover,
.edit__save:focus,
.edit__save:active {
	border: 2px solid #409eff;
}
</style>
