<script setup>
import { InfoFilled } from "@element-plus/icons-vue"

const router = useRouter()

const tasks = ref([])
const task = ref("")

const getTodos = () => {
	tasks.value = JSON.parse(localStorage.getItem("tasks"))
}

getTodos()

const saveTask = () => {
	tasks.value.push({
		id: Math.random() * 100000000000000000,
		description: task.value,
		isCompleted: false,
	})

	localStorage.setItem("tasks", JSON.stringify(tasks.value))

	router.push({ name: "index" })
}

const cancelSaveTask = () => {
	task.value = ""
	router.push({ name: "index" })
}
</script>

<template>
	<div class="container--create">
		<h2>Создать задачу</h2>
		<label class="create__label" for="create-task">Название задачи</label>
		<input
			v-model="task"
			class="create__input"
			type="text"
			placeholder=""
			id="create-task"
		/>
		<el-button-group class="create__container">
			<!-- <el-button
				@click="cancelSaveTask"
				class="create__cancel"
				type="danger"
				plain
			>
				Отменить
			</el-button> -->

			<el-popconfirm
				width="300"
				:icon="InfoFilled"
				icon-color="#ff9900"
				title="Вы уверены, что хотите отменить создание задачи?"
				@confirm="cancelSaveTask"
			>
				<template #reference>
					<el-button class="create__cancel" type="danger" plain>
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

			<el-button @click="saveTask" class="create__save" type="primary" plain>
				Сохранить
			</el-button>
		</el-button-group>
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

.container--create {
	width: 412px;
	margin: 0 auto;
	padding: 12px;
	background-color: #e6edf9;
	border-bottom: 2px solid #ffffff;
}

.container--create h2 {
	margin-bottom: 12px;
	font-weight: 400;
	font-style: normal;
	font-size: 24px;

	text-align: center;
}

.create__label {
	display: block;
	margin-bottom: 8px;
}

.create__input {
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

.create__input::placeholder {
	font-style: normal;
	font-weight: 400;
	color: black;
}

.create__input:hover {
	border-color: #c0c4cc;
}

.create__input:focus,
.create__input:active {
	border-color: #a0cfff;
}

.create__container {
	display: flex;
}

/* .create__cancel {
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

.create__cancel:hover,
.create__cancel:focus,
.create__cancel:active {
	background-color: #f56c6c;
	color: #fffff6;
	border: 2px solid #f56c6c;
} */

.create__cancel {
	border: 2px solid #facbcb;
	border-radius: 5px;
	width: 50%;
}

.create__cancel:hover,
.create__cancel:focus,
.create__cancel:active {
	border: 2px solid #f56c6c;
}

/* .create__save {
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
} */

/* .create__save:hover,
.create__save:focus,
.create__save:active {
	background-color: #409eff;
	color: #f3ffff;
	border: 2px solid #409eff;
} */

.create__save {
	border: 2px solid #a0cfff;
	border-radius: 5px;
	width: 50%;
	margin-left: 0;
}

.create__save:hover,
.create__save:focus,
.create__save:active {
	border: 2px solid #409eff;
}
</style>
