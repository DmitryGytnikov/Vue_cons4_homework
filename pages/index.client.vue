<script setup>
import { InfoFilled } from "@element-plus/icons-vue"

const router = useRouter()

const tasks = ref([
	{
		id: 0,
		description: "Изучить основы Vue",
		isCompleted: true,
	},
])

const fallbackTodos = [
	{
		id: 0,
		description: "Изучить основы Vue",
		isCompleted: false,
		subtasks: [
			{
				subId: 0,
				subDescription: "Изучить декомпозицию",
			},
			{
				subId: 1,
				subDescription: "Изучить роутинг",
			},
			{
				subId: 2,
				subDescription: "Изучить Nuxt3",
			},
			{
				subId: 3,
				subDescription: "Изучить element-plus",
			},
		],
	},
	{
		id: 1,
		description: "Подготовиться к собеседованию",
		isCompleted: true,
		subtasks: [
			{
				subId: 0,
				subDescription: "Изучить декомпозицию",
			},
			{
				subId: 1,
				subDescription: "Изучить роутинг",
			},
			{
				subId: 2,
				subDescription: "Изучить Nuxt3",
			},
			{
				subId: 3,
				subDescription: "Изучить element-plus",
			},
		],
	},
	{
		id: 2,
		description: "Устроиться на работу",
		isCompleted: false,
		subtasks: [],
	},
]

const getTodos = () => {
	if (!localStorage.getItem("tasks")) {
		localStorage.setItem("tasks", JSON.stringify(fallbackTodos))
	}

	tasks.value = JSON.parse(localStorage.getItem("tasks"))
}

getTodos()

const setCompletion = id => {
	tasks.value.find(task => task.id === id).isCompleted = !tasks.value.find(
		task => task.id === id
	).isCompleted

	localStorage.setItem("tasks", JSON.stringify(tasks.value))
}

const deleteTask = id => {
	tasks.value = tasks.value.filter(task => task.id !== id)

	localStorage.setItem("tasks", JSON.stringify(tasks.value))
}

const editTask = idTask => {
	router.push({ name: "editTodo-id", params: { id: idTask } })
}
</script>

<template>
	<!-- <ClientOnly> -->
	<div class="container--home">
		<h2>Список задач</h2>
		<NuxtLink to="/createTodo" class="create-link">
			<el-button type="success" plain>Создать задачу</el-button>
		</NuxtLink>

		<div
			v-for="(task, index) in tasks"
			:key="task.id"
			class="task"
			:class="{ 'task--completed': task.isCompleted }"
			@click="setCompletion(task.id)"
		>
			<p class="task__number">{{ index + 1 }} .</p>
			<div class="task__container">
				<p class="task__text">{{ task.description }}</p>
				<p
					v-for="(subtask, subIndex) in task.subtasks"
					:key="subtask.subId"
					class="task__subtext"
				>
					{{ subIndex + 1 }}
					<span>.</span>
					{{ subtask.subDescription }}
				</p>
			</div>
			<div>
				<el-popconfirm
					width="300"
					:icon="InfoFilled"
					icon-color="#ff9900"
					title="Вы уверены, что хотите удалить задачу?"
					@confirm.stop="deleteTask(task.id)"
				>
					<template #reference>
						<el-button @click.stop="" class="task__delete" type="danger" plain>
							Удалить
						</el-button>
					</template>
					<template #actions="{ confirm, cancel }">
						<el-button
							@click="cancel"
							plain
							style="width: 50px; border: 2px solid #409eff; border-radius: 5px"
							type="primary"
						>
							Нет
						</el-button>
						<el-button
							@click="confirm"
							plain
							style="width: 50px; border: 2px solid #409eff; border-radius: 5px"
							type="primary"
						>
							Да
						</el-button>
					</template>
				</el-popconfirm>

				<el-button
					@click.stop="editTask(task.id)"
					class="task__edit"
					type="primary"
					plain
				>
					Редактировать
				</el-button>
			</div>
		</div>
	</div>
	<!-- </ClientOnly> -->
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

.container--home {
	width: 412px;
	margin: 0 auto;
	padding: 12px;
	background-color: #e6edf9;
	border-bottom: 2px solid #ffffff;
}

.container--home h2 {
	margin-bottom: 12px;
	font-weight: 400;
	font-style: normal;
	font-size: 24px;

	text-align: center;
}

.create-link {
	display: block;
	height: 32px;

	margin-bottom: 8px;

	outline: none;
	text-decoration: none;
	text-decoration-line: none;
}

.create-link button {
	width: 100%;
	border: 2px solid #b3e19d;
	border-radius: 5px;
}

.create-link button:hover,
.create-link button:focus,
.create-link button:active {
	border: 2px solid #67c23a;
	background-color: #67c23a;
	color: #fffff3;
}

.task {
	border: 1px solid #9ca3af;
	border-radius: 5px;
	min-height: 96px;
	width: 100%;

	margin-bottom: 20px;
	padding: 12px;

	display: flex;
	align-items: center;
	cursor: pointer;
}

.task--completed {
	background-color: rgb(220 252 231);
}

.task--completed .task__text {
	text-decoration: line-through;
}

.task--completed .task__subtext {
	text-decoration: line-through;
}

.task__number {
	width: 30px;
	flex-shrink: 0;
	font-weight: bold;
	align-self: stretch;
	padding-top: 6px;
}

.task__container {
	flex-grow: 1;
	padding-right: 10px;
}

.task__text {
	font-weight: bold;
	margin-bottom: 5px;
	padding-top: 6px;
}

.task__subtext {
	margin-bottom: 5px;
}

.task div {
	flex-shrink: 0;
	align-self: stretch;
	width: 130px;

	display: flex;
	flex-direction: column;
}

.task__delete {
	width: 100%;
	border: 2px solid #facbcb;
	border-radius: 5px;
	margin-bottom: 10px;
}

.task__delete:hover,
.task__delete:focus,
.task__delete:active {
	border: 2px solid #f56c6c;
	background-color: #f56c6c;
	color: #fffff6;
}

.task__edit {
	width: 100%;
	border: 2px solid #a0cfff;
	border-radius: 5px;
	margin-left: 0;
}

.task__edit:hover,
.task__edit:focus,
.task__edit:active {
	border: 2px solid #409eff;
	background-color: #409eff;
	color: #f3ffff;
}
</style>
